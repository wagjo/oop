# Použitie LLM v aplikácii

Vyskúšame si vytvoriť aplikáciu, ktorá bude využívať LLM.

Cieľom je urobiť program, ktorý na základe zadaných ingrediencií vytvorí 3 recepty, ktoré majú dané prísady. Keďže sme ešte nebrali ako sa robí grafické rozhranie, naša aplikácia bude konzolová.

## API prístup ku LLM

Na volanie LLM z počítačového programu potrebujeme použiť tzv. API daného LLM. Existuje veľké množstvo služieb, ktoré nám poskytujú prístup k LLM API, my si dnes vyskúšame službu Groq.

!!! example "Úloha 1:"

    * Zaregistrujte sa na stránke ([https://console.groq.com](https://console.groq.com/home))
    * Prihláste sa do služby Groq a choďte na stránku s API kľúčmi [https://console.groq.com/keys](https://console.groq.com/keys)
    * Vytvorte nový kľúč s názvom filmy a s expiráciou 30 dní
    * skopírujte si zadaný kľúč. Pozor, ak si ho teraz neskopírujete, už ho viac nezískate! Klúč vyzerá napr. takto: `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`

### Projekt Varecha

Vytvoríme si projekt z existujúceho projektu na GitHube

!!! example "Úloha 2:"

    Naklonujte projekt z adresy [https://github.com/wagjo/varecha.git](https://github.com/wagjo/varecha.git)

    Vytvorte triedu sk.spse.varecha.Llm a vložte nasledovný kód:

    ```java
    package sk.spse.varecha;

    import groq4j.builders.ChatCompletionRequestBuilder;
    import groq4j.services.*;
    import java.util.*;

    public class Llm {

        private ChatService chatService;
        private ModelsService modelsService;

        public Llm(String apiKey) {
            this.chatService = ChatServiceImpl.create(apiKey);
            this.modelsService = ModelsServiceImpl.create(apiKey);
        }

        public List<String> listModels() throws Exception {
            var response = modelsService.listModels();
            List<String> models = new ArrayList<>();
            response.getActiveModels().forEach(m -> models.add(m.id()));
            Collections.sort(models);
            return models;
        }

        public String vyberModel() throws Exception {
            List<String> models = listModels();
            System.out.println("Dostupné modely:");
            for (int i = 0; i < models.size(); i++) {
                System.out.printf("  %2d. %s%n", i + 1, models.get(i));
            }

            Scanner scanner = new Scanner(System.in);
            System.out.print("Číslo modelu: ");
            String raw = scanner.nextLine().trim();
            int index = Integer.parseInt(raw) - 1;
            return models.get(index);
        }

        public String zavolajLlm(String model, String system, String user) throws Exception {
            var request = ChatCompletionRequestBuilder.create(model)
                    .systemMessage(system)
                    .userMessage(user)
                    .temperature(0.3)
                    .maxCompletionTokens(8000)
                    .build();

            var response = chatService.createCompletion(request);
            String content = response.choices().getFirst().message().content().orElse("").trim();
            return content.replace("\\n", "\n");
        }
    }
    ```

!!! example "Úloha 3:"

    Do súboru triedy `sk.spse.varecha.Varecha` vložte nasledovný kód:

    ```java
    package sk.spse.varecha;

    import java.util.ArrayList;
    import java.util.List;
    import java.util.Scanner;

    public class Varecha {

        // !!! Vložte sem svoj GROQ API kľúč !!!
        private static final String KEY = "!!! Vložte sem svoj GROQ API kľúč !!!";

        private static final String PROMPT = """
                Si expert na hľadanie a na varenie. Používateľ napíše niekoľko ingrediencií.
                Nájdi 3 recepty na jedlá, ktoré obsahujú užívateľom zadané ingrediencie.
                Odpovedz PO SLOVENSKY v tomto formáte:

                1. Názov receptu
                
                Ingrediencie:
                - prísada 1
                - prísada 2
                - ...
                Postup:
                Samotný postup receptu, zalomený na max 90 znakov na riadok, max 5 riadkov.
                
                2. ...
                
                3. ...

                Len tieto 3 recepty. Žiadny úvod, žiadny záver.
                """;

        public static String napisPrisady(Scanner scanner) {
            System.out.println();
            System.out.println("Napíš čo máš doma v chladničke, nájdem ti recept (3 ingrediencie). Prázdny riadok = odoslať.");
            System.out.println("-------------------------------------------------------");

            List<String> lines = new ArrayList<>();
            while (true) {
                String line = scanner.nextLine();
                if (line.isEmpty()) {
                    break;
                }
                lines.add(line);
            }

            String prisady = String.join("\n", lines).trim();
            if (prisady.isEmpty()) {
                System.out.println("Nič si nenapísal.");
            }
            return prisady;
        }

        public static boolean vytvorRecept(Llm llm, String model, Scanner scanner) throws Exception {
            String prisady = napisPrisady(scanner);
            if (prisady.isEmpty()) {
                return false;
            }
            System.out.println("\nHľadám 3 recepty...\n");
            String vysledok = llm.zavolajLlm(model, PROMPT, prisady);
            System.out.println(vysledok);
            return true;
        }

        public static void main(String[] args) {
            if (KEY.contains("tu vložte")) {
                System.err.println("Prosím, vložte svoj GROQ API kľúč do premennej KEY v súbore Varecha.java");
                System.exit(1);
            }

            try {
                Llm llm = new Llm(KEY);
                String model = llm.vyberModel();
                System.out.println("Vybral si si " + model);

                Scanner scanner = new Scanner(System.in);
                while (vytvorRecept(llm, model, scanner)) {
                    // pokračuj v slučke
                }
                scanner.close();
            } catch (Exception e) {
                System.err.println("Chyba: " + e.getMessage());
                e.printStackTrace();
            }
        }
    }
    ```

    * Do súboru na vyznačené miesto vložte svoj GROK API kľúč
    * Spustite program, vyberte model **qwen/qwen3.8-27b** a vyskúšajte vygenerovať recept
    * Ktorý iný model ešte funguje?

### Obmeny úlohy

!!! example "Úloha 4:"

    Vedeli by ste upraviť projekt tak, aby namiesto vymyslených receptov napísal webový odkaz na skutočný recept?

!!! example "Úloha 5:"

    Upravte váš projekt tak, aby program požiadal užívateľa o vloženie textu v nárečí alebo v nespisovnom slovenskom jazyku, s chybami
    alebo bez mäkčenov a dĺžňov a vrátil naspať pravopisne a gramaticky správny text.

    Ktorý model je naviac vhodný?

!!! example "Úloha 6:"

    Upravte váš projekt tak, aby preložil text zo slovenčiny do angličtiny.

    Ktorý model je naviac vhodný?






