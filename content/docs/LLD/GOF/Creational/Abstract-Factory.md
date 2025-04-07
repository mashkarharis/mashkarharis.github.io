 [
![](../fetched_images/Screenshot 2025-02-12 110747.png)]
The Factory Method is a creational design pattern that provides an interface for creating single objects. The Abstract Factory is also a creational design pattern, but it focuses on creating factories that produce families of related objects. In other words, an Abstract Factory is a factory of factories \(a super factory\).
Let's look at an example.
In this example, we look into UI building logic. Let's say we want to create Buttons and Checkboxes for Windows and Mac.
So, the objects are as follows:
|  1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26 |interfaceButton\{void renderButton\(\);\}interfaceCheckbox\{void renderCheckbox\(\);\}classWindowsButtonimplements Button \{publicvoidrenderButton\(\)\{
        System.out.println\("Rendering Windows Button"\);\}\}classMacButtonimplements Button \{publicvoidrenderButton\(\)\{
        System.out.println\("Rendering Mac Button"\);\}\}classWindowsCheckboximplements Checkbox \{publicvoidrenderCheckbox\(\)\{
        System.out.println\("Rendering Windows Checkbox"\);\}\}classMacCheckboximplements Checkbox \{publicvoidrenderCheckbox\(\)\{
        System.out.println\("Rendering Mac Checkbox"\);\}\} |
|---|---|
Now, let's write code to initialize these elements and render a frame for both Windows and Mac.
|  1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15 |classUI\{publicvoidwindowsFrame\(\)\{
        Button winButton =new WindowsButton\(\);
        Checkbox winCheckBox =new WindowsCheckbox\(\);
        winButton.renderButton\(\);
        winCheckBox.renderCheckbox\(\);\}publicvoidmacFrame\(\)\{
        Button macButton =new MacButton\(\);
        Checkbox macCheckBox =new MacCheckbox\(\);
        macButton.renderButton\(\);
        macCheckBox.renderCheckbox\(\);\}\} |
|---|---|
If we observe the code, we are tightly coupling the UI with the OS \(Windows/Mac\). If we want to extend Linux support, we have to add a completely new, repetitive method named linuxFrame, which also violates SOLID principles.
What if we use a higher\-level factory that generates Windows/Mac/Linux \(or whatever we support\), handles element generation logic, and injects it into the UI at runtime based on the detected OS?
This is where the Abstract Factory comes into play. Let's implement it in this example.
|  1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24 |interfaceUIFactory\{
    Button createButton\(\);
    Checkbox createCheckbox\(\);\}classWindowsUIFactoryimplements UIFactory \{public Button createButton\(\)\{returnnewWindowsButton\(\);\}public Checkbox createCheckbox\(\)\{returnnewWindowsCheckbox\(\);\}\}classMacUIFactoryimplements UIFactory \{public Button createButton\(\)\{returnnewMacButton\(\);\}public Checkbox createCheckbox\(\)\{returnnewMacCheckbox\(\);\}\} |
|---|---|
As we can see, we now have higher\-level UI factories responsible for handling OS\-specific elements.
Now, we can decouple the UI factory from OS\-based logic as follows:
|  1
 2
 3
 4
 5
 6
 7
 8
 9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24 |classUI\{
    UIFactory uiFactory;publicUI\(UIFactory uiFactory\)\{this.uiFactory= uiFactory;\}publicvoidgenerateFrame\(\)\{
        Checkbox checkBoxForAccept = uiFactory.createCheckbox\(\);
        Button buttonForSubmit = uiFactory.createButton\(\);
        checkBoxForAccept.renderCheckbox\(\);
        buttonForSubmit.renderButton\(\);\}\}classAbstractFactoryExample\{publicstaticvoidmain\(String\[\] args\)\{
        UI windowsUI =new UI\(new WindowsUIFactory\(\)\);
        windowsUI.generateFrame\(\);

        UI macUI =new UI\(new MacUIFactory\(\)\);
        macUI.generateFrame\(\);\}\} |
|---|---|
Output:
| 1
2
3
4 |Rendering Windows Checkbox  
Rendering Windows Button  
Rendering Mac Checkbox  
Rendering Mac Button  
 |
|---|---|
***
***
***When to use the Abstract Factory?***
***
***
* When we want to generate families of grouped elements \(Windows element family, Mac element family\).
* To decouple the generation of families of elements from the implementation logic.

***
***
***Difference between Factory Method and Abstract Factory***
***
***
* Factory Method : A method that returns an instance of a single product.
* Abstract Factory : A factory that returns multiple related products.

***Class Diagram******
***
[
![](../fetched_images/abstract_factory.drawio.png)]
Happy Coding 🙌
