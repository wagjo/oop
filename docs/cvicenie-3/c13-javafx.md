# Cvičenie 13: JavaFX

Na tomto cvičení si nainštalujeme a vyskúšame JavaFX

## JavaFX

GUI knižnica JavaFX v minulosti bola súčasťou JDK, dnes je však k dispozícii iba samostatne. Hlavná stránka projektu JavaFX je [https://openjfx.io/](https://openjfx.io/). Na tejto stránke si viete stiahnuť najnovšiu verziu.

Verzie JavaFX sú vydávané a číslované rovnako ako verzie Javy. Preto ak používame Javu 21, je potrebné stiahnuť **JavaFX tiež verzie 21**, nie novšie.

JavaFX nie je iba obyčajná knižnica, ale obsahuje aj platformovo špecifické súbory, teda má inú verziu pre windows, inú pre Linux, atď. Jej inštalácia a používanie v Java programoch teda nie je úplne triviálne.

## JavaFX pomocou Maven

Pomocou Maven je možné veľmi jednoducho používať JavaFX vo svojich projektoch.

Jediné, čo je potrebné, je pridať nasledovné závislosti do vášho projektu:

- JavaFX Controls - `org.openjfx:javafx-controls:21.0.10` - základné a najpoužívanejšie balíky
- JavaFX FXML - `org.openjfx:javafx-fxml:21.0.10` - skriptovací formát pre pohľady

!!! example "Úloha 13.1: JavaFX projekt"

    V IntelliJ IDEA si vytvorte nový Java projekt

    1. Pridajte do neho `javafx-controls` maven knižnicu podľa vyššie uvedených inštrukcií (File-Project Structure-Libraries)

    1. Vytvorte triedu `sk.spse.counter.FxCounterApplication` s nasledovným kódom:

        ```java
        package sk.spse.counter;

        import javafx.application.Application;
        import javafx.geometry.Insets;
        import javafx.scene.Scene;
        import javafx.scene.control.Button;
        import javafx.scene.control.Label;
        import javafx.scene.control.TextField;
        import javafx.scene.layout.HBox;
        import javafx.stage.Stage;

        // Hlavná GUI trieda musí dediť z triedy Application
        public class FxCounterApplication extends Application {

            // Aktuálna hodnota počítadla
            private int counter = 0;

            // Hlavná metóda, ktorá sa volá pri spustení JavaFX aplikácie
            @Override
            public void start(Stage stage) {
                // 1. Vytvoríme textový popisok (Label)
                Label label = new Label("Počítadlo:");
                // Nastavíme veľkosť písma inline pomocou CSS
                label.setStyle("-fx-font-size: 18px;");

                // 2. Textové pole, ktoré zobrazuje aktuálnu hodnotu počítadla
                TextField textField = new TextField("0");

                // Používateľ nebude môcť priamo písať do poľa
                textField.setEditable(false);

                // Šírka poľa – približne pre 4 znaky
                textField.setPrefColumnCount(4);

                // Väčšie písmo + vnútorné odsadenie pre krajší vzhľad
                textField.setStyle("""
                    -fx-font-size: 18px;
                    -fx-padding: 10 20 10 20;
                """);

                // 3. Tlačidlo, ktoré zvyšuje hodnotu počítadla
                Button button = new Button("Plus 1");
                button.setStyle("-fx-font-size: 18px;");

                // Definícia, čo sa stane po kliknutí na tlačidlo (lambda výraz)
                button.setOnAction(e -> {
                    counter++;                      // zvýšime hodnotu
                    textField.setText(String.valueOf(counter));  // aktualizujeme zobrazenie
                });

                // 4. Vytvoríme horizontálny layout (prvky idú vedľa seba)
                // parameter 20 = medzera medzi prvkami
                HBox root = new HBox(20, label, textField, button);

                // Vonkajšie odsadenie celého obsahu od okrajov okna
                root.setPadding(new Insets(20));

                // 5. Vytvoríme scénu a vložíme do nej náš layout (root)
                Scene scene = new Scene(root);

                // Nastavenia okna (Stage)
                stage.setTitle("JavaFX Counter");
                stage.setScene(scene);
                stage.setResizable(true); // používateľ môže meniť veľkosť okna

                // Zobrazíme okno
                stage.show();
            }
        }
        ```

    1. Keďže JavaFX majú problém, ak je metóda `main` priamo v hlavnej triede programu, vytvoríme si samostatnú triedu `sk.spse.counter.FxCounterMain` s nasledovným kódom:

        ```java
        package sk.spse.counter;

        public class FxCounterMain {

            public static void main(final String[] args) {
                FxCounterApplication.launch(FxCounterApplication.class, args);
            }

        }
        ```

    Spustite aplikáciu a overte jej funkčnosť

## FXML

V príklade, ktorý ste si vyskúšali, sme vytvorili jednoduché počítadlo. Všetky grafické komponenty sme vytvorili priamo v kóde. Naviac sme v kóde priamo uviedli aj veľkosti a pozície týchto komponentov. Dokonca sme v kóde napísali aj štýly pre jednotlivé komponenty (veľkosti písma, farby, ...). 

Takýto postup je veľmi neefektívny a vytvára neprehľadný kód, ktorý sa ťažko udržiava. JavaFX nám ponúka vytvoriť rozloženie prvkov a ich štýly mimo kódu. Samostatné rozloženie komponentov sa v JavaFX robí pomocou tzv. FXML súboru, čo je niečo ako HTML súbor pre GUI aplikácie. Tento súbor je vo formáte XML, teda aj jazyk je podobný HTML. 

!!! example "Úloha 13.2: Počítadlo pomocou FXML"

    Teraz si vytvoríme počítadlo pomocou FXML. Budeme vytvárať 3 triedy a jeden FXML súbor

    1. Do projektu pridajte `javafx-fxml` maven knižnicu podľa vyššie uvedených inštrukcií (File-Project Structure-Libraries)

    2. V adresári kde máte vaše triedy (sk.spse.counter) vytvorte súbor `counter-view.fxml` a vložte do neho nasledovný kód

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>

        <?import javafx.geometry.Insets?>
        <?import javafx.scene.control.Button?>
        <?import javafx.scene.control.Label?>
        <?import javafx.scene.control.TextField?>
        <?import javafx.scene.layout.HBox?>

        <!--
        Hlavný kontajner – horizontálne usporiadanie prvkov
        fx:controller ukazuje na triedu, ktorá bude spravovať logiku
        -->
        <HBox xmlns="http://javafx.com/javafx/21"
            xmlns:fx="http://javafx.com/fxml/1"
            fx:controller="sk.spse.counter.FxCounterController"
            alignment="CENTER"
            spacing="20">

        <padding>
            <Insets topRightBottomLeft="10" />
        </padding>

        <!-- Popisok -->
        <Label text="Počítadlo:"
            style="-fx-font-size: 18px;" />

        <!-- Textové pole na zobrazenie hodnoty (nebude editovateľné) -->
        <TextField fx:id="counterField"
                text="0"
                editable="false"
                prefColumnCount="4"
                style="-fx-font-size: 18px; -fx-padding: 10 20 10 20;" />

        <!-- Tlačidlo na zvýšenie hodnoty -->
        <Button text="Plus 1"
                style="-fx-font-size: 18px;"
                onAction="#incrementCounter" />

        </HBox>
        ```

    1. Vytvorte triedu `sk.spse.counter.FxCounterController` s nasledovným kódom

        ```java
        package sk.spse.counter;

        import javafx.fxml.FXML;
        import javafx.scene.control.TextField;

        /**
         * Controller pre FXML súbor – obsahuje logiku počítadla
         */
        public class FxCounterController {

            // Premenná počítadla (začína na 0)
            private int counter = 0;

            // Odkaz na TextField z FXML (musí mať rovnaké fx:id)
            @FXML
            private TextField counterField;

            /**
             * Metóda, ktorá sa automaticky zavolá po kliknutí na tlačidlo +
             * (onAction="#incrementCounter" v FXML)
             */
            @FXML
            private void incrementCounter() {
                counter++;                           // zvýšime počítadlo
                counterField.setText(String.valueOf(counter));  // aktualizujeme zobrazenie
            }
        }
        ```

    1. Vytvorte triedu `sk.spse.counter.FxCounterFxmlApplication` s kódom

        ```java
        package sk.spse.counter;

        import javafx.application.Application;
        import javafx.fxml.FXMLLoader;
        import javafx.scene.Parent;
        import javafx.scene.Scene;
        import javafx.stage.Stage;

        import java.io.IOException;

        public class FxCounterFxmlApplication extends Application {

            @Override
            public void start(Stage stage) throws IOException {
                // Načítame FXML súbor
                FXMLLoader fxmlLoader = new FXMLLoader(FxCounterFxmlApplication.class.getResource("counter-view.fxml"));

                // root bude HBox definovaný vo FXML
                Parent root = fxmlLoader.load();

                // Vytvoríme scénu
                Scene scene = new Scene(root);

                // Nastavenia okna
                stage.setTitle("JavaFX FXML Counter");
                stage.setScene(scene);
                stage.setResizable(true);

                // Zobrazíme okno
                stage.show();
            }
        }        
        ```

    1. Vytvorte triedu `sk.spse.counter.FxCounterFxmlMain` s kódom

        ```java
        package sk.spse.counter;

        public class FxCounterFxmlMain {

            public static void main(final String[] args) {
                FxCounterFxmlApplication.launch(FxCounterFxmlApplication.class, args);
            }

        }
        ```

    Spustite aplikáciu pomocou triedy FxCounterFxmlMain a overte funkčnosť programu.
    
Teraz máme súborov síce viac, ale sú prehľadne rozdelené:

- Vizuálna stránka - `counter-view`
- Správanie - `FxCounterController`
- Hlavná aplikácia - `FxCounterFxmlApplication`

Všetok kód týkajúci sa rozloženia (layout) a štýlov (look and feel) je teraz v FXML súbore.

## Scene Builder

Ručné písanie FXML je opäť dosť nepraktické, preto nám JavaFX poskytuje editor FXML súborov s názvom Scene Builder.

Scene Builder je samostatná aplikácia, ktorú treba stiahnuť a nainštalovať zo stránky [https://gluonhq.com/products/scene-builder/#download](https://gluonhq.com/products/scene-builder/#download)

!!! example "Úloha 13.3: Inštalácia Scene Buildera"

    1. Stiahnite si Scene Builder zo stránky [https://gluonhq.com/products/scene-builder/#download](https://gluonhq.com/products/scene-builder/#download)

    2. Nainštalujte Scene Builder do svojho počítača. POZOR!!!: Zapamätajte si presnú cestu, kde sa Scene Builder nainsštaloval

    3. Choďte do nastavení (Settings) IntelliJ IDEA a nastavte cestu do Scene Buildera v menu `Language & Frameworks - JavaFX - Path to Scene Builder`

    4. Kliknite na FXML súbor pravým tlačítkom a vyberte možnosť `Open in Scene Builder`

    Overte, že sa vám otvorí scene builder s oknom počítadla
