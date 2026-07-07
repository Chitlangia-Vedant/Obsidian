We can create a simple window using JavaFX with the following program.

```java
package application;

import javafx.application.Application;
import javafx.stage.Stage;

public class JavaFxApplication extends Application {

    @Override
    public void start(Stage window) {
        window.setTitle("Hello World!");
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxApplication.class);
    }
}
```

When the program is launched, it looks as follows.
![[Pasted image 20260223143004.png]]

How the above program works:
1. When the `launch` method is called, the method of the Application class creates a new object from the given class (here `JavaFxApplication`) and calls its `init` method. 
2. The `init` method is defined in the Application class and is used, for instance, to initialize objects of a program. 
3. After calling the `init` method, the program calls the start method, which gets a [Stage](https://docs.oracle.com/javase/8/javafx/api/javafx/stage/Stage.html) object as its parameter, which describes the window. In the implementation of the `start` method above, the `setTitle` method sets the title of the Stage-type window object obtained as a parameter. 
4. The method `show` is then called, which leads to the window being displayed. 
5. The program then stays in a state where it continuously listens to events on user interface, such as closing the window that causes the application to shut down.

# Structure of a User Interface

<u>Graphical user interfaces consist of three essential parts. </u>
1. The Stage object behaves as the program's window.
2. A [Scene](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/Scene.html) is set for a Stage object that represents a scene within the window. 
3. The Scene object, on the other hand, contains an object responsible for arranging the components belonging to the scene (such as FlowPane), which contains the actual user interface components.

The program below creates an interface with a single button.

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
        Button button = new Button("This is a button");

        FlowPane componentGroup = new FlowPane();
        componentGroup.getChildren().add(button);

        Scene scene = new Scene(componentGroup);

        window.setScene(scene);
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxApplication.class);
    }
}
```

The application looks like this.
![[Pasted image 20260223144347.png]]

UI components are added as "children" to the object responsible for setting them — `FlowPane`. 
This has to do with a `JavaFx` design decision, whereby each object responsible for UI components may contain other objects responsible for UI components as well as actual UI components.
This enables GUIs where the layout of the UI components depends on their location on the user interface. 
For example, menu items located at the top of a UI are usually placed side by side, while list items are placed one below the other.

**To briefly summarize, the UI structure is as follows. 
The window contains a Scene object. → The Scene object contains the object responsible for the layout of the user-interface components. → The object responsible for the component layout can contain both UI components and objects responsible for UI component layouts.**