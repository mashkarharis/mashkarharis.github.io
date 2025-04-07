 
[
![](../fetched_images/Screenshot 2025-02-20 165133.png)]In our application there can be Objects, which takes lot of resources and effort to initialize and build up to some extent. These types of objects creation are costly that requires a lot of time and resources. But if we already created a such Object, if we want more Objects like, what we do ? create from scratch again or using same Object to just clone it. It is the design decision, which involves Prototype Design Pattern. Prototype Design Pattern is mechanism to copy the original object to a new object and modify it according to our needs.
Let's start with an example.
Let's say we get a request from user, where user uploads a image file, and our system is suppose to send PNG and JPEG version of that image file. Let's build flow for two approaches we have,
First Option
* Get file from user
* Create 2 objects \(with same user file content\) for each file type that we suppose to send \(consumes more memory\)
* Send each object to file scanner and scan for vulnerabilities \(costly task and invoked 2 times\)
* Convert each scanned objects to preferred target file type

[
![](../fetched_images/image1.png)]
**OR**
Second Option
* Get file from user
* Put file content into 1 Object \(now less memory used\)
* Send that object to scan and  Get results \(costly task but invoked only 1 time\)
* Now clone into 2 copies for we needed \(PNG & JPEG\)
* Convert and send results

[
![](../fetched_images/image1.png)]
As we can see second approach is more sound and efficient. However it need some mechanism to clone object. This is where we can use protype design pattern.
As we concluded The better approach would be to clone the existing object into a new object and then do the data manipulation to reduce number of times costly tasks invocation and save more memory.
When is come to prototyping there is two ways we can do
1. Shallow Copy : Creates a new instance of the object but shares references of nested objects \(if any\), Changes in nested objects affect both the original and the cloned object. Primitive fields like int, double, boolean, etc., are not shared in a shallow copy.
2. Deep copy : Creates a new instance of the object and its nested objects. Make sure that changes in the original object, including nested objects, do not affect the cloned object and its nested objects.

[
![](../fetched_images/image1.png)]
### Simple Coding Example \(JAVA\)
#### 

#### Shallow Copy
|   1
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
 26
 27
 28
 29
 30
 31
 32
 33
 34
 35
 36
 37
 38
 39
 40
 41
 42
 43
 44
 45
 46
 47
 48
 49
 50
 51
 52
 53
 54
 55
 56
 57
 58
 59
 60
 61
 62
 63
 64
 65
 66
 67
 68
 69
 70
 71
 72
 73
 74
 75
 76
 77
 78
 79
 80
 81
 82
 83
 84
 85
 86
 87
 88
 89
 90
 91
 92
 93
 94
 95
 96
 97
 98
 99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
122
123
124
125
126
127
128
129
130
131
132
133
134
135
136
137
138
139
140
141
142
143
144
145
146
147
148
149
150
151
152
153
154
155
156
157
158 |package Example;importjava.io.File;importjava.time.LocalDateTime;importjava.util.HashMap;publicclassPrototypeDesignPattern\{publicstaticvoidmain\(String\[\] args\)throws CloneNotSupportedException \{// DEFINE ORIGINAL IMAGE AND PRINT
        ImageFile ImageOriginal =new ImageFile\(new File\("C:\\Users\\HarisMohomedAshkar\\Downloads\\Test.png"\)\);
        ScanResult scanResult =new ScanResult\(true, LocalDateTime.now\(\)\);
        ImageOriginal.addMetaData\("Tag","TestImage\_1"\);
        ImageOriginal.setScanResult\(scanResult\);
        System.out.println\(ImageOriginal.toString\(\)\);// ImageFile\{// 		size=35445, filePath=C:\Users\HarisMohomedAshkar\Downloads\Test.png, //      metaData=\{path=C:\Users\HarisMohomedAshkar\Downloads\Test.png, name=Test.png, Tag=TestImage\_1\}, //      scanResult=ScanResult\{isSafe=true, scannedAt=2025\-02\-14T11:10:29.946809300\}// \}// CLONE AND PRINT
        ImageFile ImageClone = ImageOriginal.clone\(\);
        System.out.println\(ImageClone.toString\(\)\);// ImageFile\{// 		size=35445, filePath=C:\Users\HarisMohomedAshkar\Downloads\Test.png, //      metaData=\{path=C:\Users\HarisMohomedAshkar\Downloads\Test.png, name=Test.png, Tag=TestImage\_1\}, //      scanResult=ScanResult\{isSafe=true, scannedAt=2025\-02\-14T11:10:29.946809300\}// \}// UPDATE SOME NESTED OBJECTS IN ORIGINAL
        scanResult.setSafe\(false\);
        scanResult.setScannedAt\(LocalDateTime.now\(\)\);// ORIGINAL AFTER UPDATED
        System.out.println\(ImageOriginal.toString\(\)\);// ImageFile\{// 		size=35445, filePath=C:\Users\HarisMohomedAshkar\Downloads\Test.png, //      metaData=\{path=C:\Users\HarisMohomedAshkar\Downloads\Test.png, name=Test.png, Tag=TestImage\_1\}, //      scanResult=ScanResult\{isSafe=false, scannedAt=2025\-02\-14T11:14:07.838987400\}// \}// CLONE AFTER UPDATED
        System.out.println\(ImageClone.toString\(\)\);// ImageFile\{// 		size=35445, filePath=C:\Users\HarisMohomedAshkar\Downloads\Test.png, //      metaData=\{path=C:\Users\HarisMohomedAshkar\Downloads\Test.png, name=Test.png, Tag=TestImage\_1\}, //      scanResult=ScanResult\{isSafe=false, scannedAt=2025\-02\-14T11:14:07.838987400\}// \}// CHANGES IS NESTED OBJECTS IN ORIGINAL IMAGEFILE, AFFECTS CLONED IMAGEFILE\}\}classImageFileimplements Cloneable \{privatelong size;private File file;private HashMap < String, String > metaData;private ScanResult scanResult;publicImageFile\(File file\)\{this.size= file.length\(\);this.file= file;this.metaData=new HashMap < String, String >\(\);this.metaData.put\("name", file.getName\(\)\);this.metaData.put\("path", file.getPath\(\)\);\}publiclonggetSize\(\)\{return size;\}public File getFile\(\)\{return file;\}public HashMap < String, String > getMetaData\(\)\{return metaData;\}public ScanResult getScanResult\(\)\{return scanResult;\}public ImageFile addMetaData\(String key, String val\)\{this.metaData.putIfAbsent\(key, val\);returnthis;\}public ImageFile setMetaData\(HashMap < String, String > metaData\)\{this.metaData= metaData;returnthis;\}public ImageFile setScanResult\(ScanResult scanResult\)\{this.scanResult= scanResult;returnthis;\}@Overridepublic String toString\(\)\{return"ImageFile\{"\+"size="\+ size \+", filePath="\+ file.getAbsolutePath\(\)\+", metaData="\+ metaData \+", scanResult="\+ scanResult \+'\}';\}@Overrideprotected ImageFile clone\(\)throws CloneNotSupportedException \{// Use same File, ScanResult and MetaData References
        ImageFile imageFile =new ImageFile\(file\);
        imageFile
            .setScanResult\(scanResult\).setMetaData\(metaData\);return imageFile;\}\}classScanResult\{privateboolean isSafe;private LocalDateTime scannedAt;publicScanResult\(boolean isSafe, LocalDateTime scannedAt\)\{this.isSafe= isSafe;this.scannedAt= scannedAt;\}publicbooleanisSafe\(\)\{return isSafe;\}public LocalDateTime getScannedAt\(\)\{return scannedAt;\}publicvoidsetSafe\(boolean isSafe\)\{this.isSafe= isSafe;\}publicvoidsetScannedAt\(LocalDateTime scannedAt\)\{this.scannedAt= scannedAt;\}@Overridepublic String toString\(\)\{return"ScanResult\{"\+"isSafe="\+ isSafe \+", scannedAt="\+ scannedAt \+'\}';\}\} |
|---|---|
#### Deep Copy
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
11 |@Overrideprotected ImageFile clone\(\)throws CloneNotSupportedException \{// Create a new Objects For File, ScanResult and MetaData
        File newFile =new File\(file.getAbsolutePath\(\)\);
        ScanResult newScanResult =new ScanResult\(scanResult.isSafe\(\), LocalDateTime.from\(scanResult.getScannedAt\(\)\)\);
        HashMap < String, String > newMetaData =new HashMap <>\(metaData\);
        ImageFile clonedImageFile =new ImageFile\(newFile\);
        clonedImageFile.setScanResult\(newScanResult\).setMetaData\(newMetaData\);return clonedImageFile;\} |
|---|---|
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
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50 |publicclassPrototypeDesignPattern\{publicstaticvoidmain\(String\[\] args\)throws CloneNotSupportedException \{// DEFINE ORIGINAL IMAGE AND PRINT
        ImageFile ImageOriginal =new ImageFile\(new File\("C:\\Users\\HarisMohomedAshkar\\Downloads\\Test.png"\)\);
        ScanResult scanResult =new ScanResult\(true, LocalDateTime.now\(\)\);
        ImageOriginal.addMetaData\("Tag","TestImage\_1"\);
        ImageOriginal.setScanResult\(scanResult\);
        System.out.println\(ImageOriginal.toString\(\)\);// ImageFile\{// 		size=35445, filePath=C:\Users\HarisMohomedAshkar\Downloads\Test.png, //      metaData=\{path=C:\Users\HarisMohomedAshkar\Downloads\Test.png, name=Test.png, Tag=TestImage\_1\}, //      scanResult=ScanResult\{isSafe=true, scannedAt=2025\-02\-14T11:27:56.571472200\}// \}// CLONE AND PRINT
        ImageFile ImageClone = ImageOriginal.clone\(\);
        System.out.println\(ImageClone.toString\(\)\);// ImageFile\{// 		size=35445, filePath=C:\Users\HarisMohomedAshkar\Downloads\Test.png, //      metaData=\{name=Test.png, path=C:\Users\HarisMohomedAshkar\Downloads\Test.png, Tag=TestImage\_1\}, //      scanResult=ScanResult\{isSafe=true, scannedAt=2025\-02\-14T11:27:56.571472200\}// \}// UPDATE SOME NESTED OBJECTS IN ORIGINAL
        scanResult.setSafe\(false\);
        scanResult.setScannedAt\(LocalDateTime.now\(\)\);// ORIGINAL AFTER UPDATED
        System.out.println\(ImageOriginal.toString\(\)\);// ImageFile\{// 		size=35445, filePath=C:\Users\HarisMohomedAshkar\Downloads\Test.png, //      metaData=\{path=C:\Users\HarisMohomedAshkar\Downloads\Test.png, name=Test.png, Tag=TestImage\_1\}, //      scanResult=ScanResult\{isSafe=false, scannedAt=2025\-02\-14T11:27:56.590477700\}// \}// CLONE AFTER UPDATED
        System.out.println\(ImageClone.toString\(\)\);// ImageFile\{// 		size=35445, filePath=C:\Users\HarisMohomedAshkar\Downloads\Test.png, //      metaData=\{path=C:\Users\HarisMohomedAshkar\Downloads\Test.png, name=Test.png, Tag=TestImage\_1\}, //      scanResult=ScanResult\{isSafe=true, scannedAt=2025\-02\-14T11:27:56.571472200\}// \}// CHANGES IS NESTED OBJECTS IN ORIGINAL IMAGEFILE & CLONED IMAGEFILE ARE INDEPENDENT\}\} |
|---|---|
Here we have looked how we can use prototype design pattern and clone a shallow or deep copy for an existing objects to make our application more efficient.
Happy Coding 🙌
