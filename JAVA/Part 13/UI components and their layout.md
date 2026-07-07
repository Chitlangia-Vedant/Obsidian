## Label
Text can be displayed using the [Label](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/control/Label.html) class. The Label class provides a UI component for which text can be set, and offers methods for modifying the text it contains. The displayed text is either set in the constructor, or by using the `setText` method.

```java
package application;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.layout.FlowPane;
import javafx.stage.Stage;

public class JavaFxApplication extends Application {

    @Override
    public void start(Stage window) {
        Label textComponent = new Label("Text element");

        FlowPane componentGroup = new FlowPane();
        componentGroup.getChildren().add(textComponent);

        Scene view = new Scene(componentGroup);

        window.setScene(view);
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxApplication.class);
    }
}
```

[![Window with a textComponent. The window shows the text 'Text element'.](https://java-programming.mooc.fi/static/dae4da69f87e9eead5dde9cedb6dab1e/0ec92/gui-tekstielementti.png "Window with a textComponent. The window shows the text 'Text element'.")](https://java-programming.mooc.fi/static/dae4da69f87e9eead5dde9cedb6dab1e/0ec92/gui-tekstielementti.png)
## Button
Buttons can be added using the [Button](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/control/Button.html) class. 
```java
package application;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.FlowPane;
import javafx.stage.Stage;

public class JavaFxApplication extends Application {

    @Override
    public void start(Stage window) {
        Button buttonComponent = new Button("This is a button");

        FlowPane componentGroup = new FlowPane();
        componentGroup.getChildren().add(buttonComponent);

        Scene view = new Scene(componentGroup);

        window.setScene(view);
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxApplication.class);
    }
}
```

[![Ikkuna, jossa on nappi. Napissa on teksti 'This is a button'.](https://java-programming.mooc.fi/static/d17ded0d4cd4d946ee47382f1b05086d/0ec92/gui-nappi.png "Ikkuna, jossa on nappi. Napissa on teksti 'This is a button'.")](https://java-programming.mooc.fi/static/d17ded0d4cd4d946ee47382f1b05086d/0ec92/gui-nappi.png)

## Multiple Component
You also have the ability to add multiple components at once. In the example below, both a button and a textComponent have been added.

```java
package application;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.FlowPane;
import javafx.stage.Stage;

public class JavaFxApplication extends Application {

    @Override
    public void start(Stage window) {
        Button buttonComponent = new Button("This is a button");
        Label textComponent = new Label("Text element");

        FlowPane componentGroup = new FlowPane();
        componentGroup.getChildren().add(buttonComponent);
        componentGroup.getChildren().add(textComponent);

        Scene view = new Scene(componentGroup);

        window.setScene(view);
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxSovellus.class);
    }
}
```

The application looks like this:

[![Ikkuna, jossa on nappi sekä textComponent. Napissa on teksti 'This is a button' ja textComponent sisältää tekstin 'Text element'.](https://java-programming.mooc.fi/static/61d3eb38fe5cefc6b7b39fda5d0e50e9/0ec92/gui-nappi-ja-teksti.png "Ikkuna, jossa on nappi sekä textComponent. Napissa on teksti 'This is a button' ja textComponent sisältää tekstin 'Text element'.")](https://java-programming.mooc.fi/static/61d3eb38fe5cefc6b7b39fda5d0e50e9/0ec92/gui-nappi-ja-teksti.png)

You can find a list of the available UI components on [https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/](https://docs.oracle.com/javase/8/javafx/user-interface-tutorial/).

# UI Component Layout
## Flowplane
With `FlowPane`, components that you add to the interface are placed side-by-side. 
If the size of Window is reduced so that the components no longer fit next to each other, the components will be automatically aligned.

![[Pasted image 20260223170521.png]]
The application has been narrowed so that the components are automatically aligned.

## BorderPane
The [BorderPane](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/layout/BorderPane.html) class lets you lay out components in five different primary positions: top, right, bottom, left and center

```java
package application;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;

public class JavaFxSovellus extends Application {

    @Override
    public void start(Stage window) {
        BorderPane layout = new BorderPane();
        layout.setTop(new Label("top"));
        layout.setRight(new Label("right"));
        layout.setBottom(new Label("bottom"));
        layout.setLeft(new Label("left"));
        layout.setCenter(new Label("center"));

        Scene view = new Scene(layout);

        window.setScene(view);
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxSovellus.class);
    }
}
```

![[Pasted image 20260223171355.png]]

