---
# 786
weight: 300
---
# GOF Design Patterns : Composite

## Introduction

According to GOF, Composite Design Pattern falls under Structural Design Patterns. 
Because this pattern is more about, how we compose hierachical object sturcture to process data effiiently.

According to GOF definition,
```text
Compose objects into tree structure to represent part-whole hierarchies. Composite lets client treat individual objects and compositions of objects uniformly
```

## When to Use

This pattern is used in cases which has,
- Higherarchical Object storage, where Object holds other objects in next level of hierachy like a tree structure
- Client programme uses only common method across all objects in the hierachy, and process each nodes similarly.

## Example

One of the best example we can us this pattern is how we can process FileStorage in a System.

FileStorage usually have two types of Objects,

- File : Single Unit of Object in the Hierachy with no childs (We call this as leaf)
- Folder : Compostie Object Which can hold Other child Objects (Single / Composite) in the Hierachy (We call this Composite)

As we know there are lot of methods which is common to files and folders. FOr our programme we use display(), getSize(), displayList() methods,
which is common to Both Files and FOlders. Functions are as below,

- displayList() : Return the structure and size of elements in underlying tree as string array
- display() : Print the structure and size of elements in underlying tree
- getSize() : Return total size of all undelying elements in the tree

### Component
First we need to define and interface to store this common methods. This inerface is called as "Component".
```java
sealed interface StorageEntity permits File, Folder {

	List<String> displayList();

	double getSize();

	void display();

}
```
### Leaf
Then we need to define our leaf object, which is the simplest unit, which does not hold children. In our example, this is `File`.
```java
final class File implements StorageEntity {

	String name;
	double size;

	public File(String name, double size) {
		this.name = name;
		this.size = size;
	}

	@Override
	public List<String> displayList() {
		return List.of("- " + this.name + " : " + new DecimalFormat("##.00").format(getSize()) + " KB");
	}

	@Override
	public double getSize() {
		return this.size;
	}

	@Override
	public void display() {
		System.out.println(displayList().get(0));
	}

}
```
### Composite
Next we will define our composite object, which processes it self and all it's children
```java
final class Folder implements StorageEntity {

	String name;
	private List<StorageEntity> entities;

	public Folder(String name) {
		this.name = name;
		this.entities = new ArrayList<StorageEntity>();
	}

	@Override
	public List<String> displayList() {
		List<String> lines = new ArrayList<String>();
		lines.add("[D] " + this.name + " [T] " + new DecimalFormat("##.00").format(getSize()) + " KB");
		for (StorageEntity storageEntity : entities) {
			for (String line : storageEntity.displayList()) {
				lines.add("    " + line);
			}
		}
		return lines;
	}

	@Override
	public double getSize() {
		double sumSize = 0;
		for (StorageEntity storageEntity : entities) {
			sumSize = sumSize + storageEntity.getSize();
		}
		return sumSize;
	}

	@Override
	public void display() {
		for (String line : displayList()) {
			System.out.println(line);
		}
	}

	public void addEntity(StorageEntity entity) {
		this.entities.add(entity);
	}

}
```
### Client
Now let's build some simple storage hierachy to test our Composite Design Pattern example.
```java
class Client {

	public static void main(String[] args) {

		File beach = new File("beach.png", 11.2);
		File dolphin = new File("dolphin.png", 12.4);
		File oak = new File("oak.png", 10.1);
		File mango = new File("mango.png", 9.6);
		File eagle = new File("eagle.jpeg", 1.1);

		File ss1 = new File("ss1.jpeg", 1.1);
		File ss2 = new File("ss2.jpeg", 2.3);
		File ss3 = new File("ss3.jpeg", 1.7);

		Folder root = new Folder("ROOT");
		Folder cameraRoll = new Folder("CAMERA ROLL");
		Folder screenShot = new Folder("SCREENSHOTS");
		Folder tree = new Folder("TREE");
		Folder bird = new Folder("BIRD");
		Folder fish = new Folder("FISH");

		root.addEntity(cameraRoll);
		root.addEntity(screenShot);

		cameraRoll.addEntity(fish);
		cameraRoll.addEntity(tree);
		cameraRoll.addEntity(bird);
		cameraRoll.addEntity(beach);

		screenShot.addEntity(ss1);
		screenShot.addEntity(ss2);
		screenShot.addEntity(ss3);

		fish.addEntity(dolphin);
		tree.addEntity(oak);
		tree.addEntity(mango);
		bird.addEntity(eagle);

		root.display();

	}

}
```
### Output
Here is what we will get as our output,
```text
[D] ROOT [T] 49.50 KB
    [D] CAMERA ROLL [T] 44.40 KB
        [D] FISH [T] 12.40 KB
            - dolphin.png : 12.40 KB
        [D] TREE [T] 19.70 KB
            - oak.png : 10.10 KB
            - mango.png : 9.60 KB
        [D] BIRD [T] 1.10 KB
            - eagle.jpeg : 1.10 KB
        - beach.png : 11.20 KB
    [D] SCREENSHOTS [T] 5.10 KB
        - ss1.jpeg : 1.10 KB
        - ss2.jpeg : 2.30 KB
        - ss3.jpeg : 1.70 KB
```
## Wrapup
As discussed so far this pattern is more about handling tree like hiearchical strcutre to,
- create complex structures dynamically and process them more efficiently.
- reduce lot of duplicate codes by allowing use process each element through common component interface.
- create hiearchy once and process multiple ways as we needed, which saves memeory and ttime for recreation and allocate new objects each time.

Hope this article was helpful.
Thank you !

Happy Coding 🙌