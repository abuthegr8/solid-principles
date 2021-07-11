# SOLID PRINCIPLES

## Introduction

SOLID is an acronym for the first five object-oriented design (OOD) principles by Robert C. Martin.
<br>

These principles establish practices that lend to developing software with considerations for maintaining and extending as the project grows.
These principles are necessary for avoiding deep code problems, refactoring code, and agile or adaptive software development.
<br>

SOLID stands for five principles, namely:

   1. S - Single-Responsiblity Principle
   2. O - Open-Closed Principle
   3. L - Liskov Substitution Principle
   4. I - Interface Segregation Principle
   5. D - Dependency Inversion Principle
<br><br>

## 1. Single-Responsiblity Principle (SRP)

Single-responsibility Principle (SRP) states:

    A class should have one and only one reason to change, meaning that a class should have only one job.

This means that all classes, modules, functions pr any other similar block of code should have a single responsibility and definition of what a class does should be done in a single line.

For example, take the following code into consideration:

```dart
class ComputerOperations{
    int config = 0;//default config

    Start(){
        print("Computer Started.");
        config = 1;
    }

    End(){
        print("Computer Shut Down.");
        config = 0;
    }
}
```

Here we have a class `ComputerOperations` that has two methods/responsiblities called `Start` and `End`. Therefore, this class has two reasons to change: one if `Start` function configuration is changed and two if if `End` function configuration is changed. Hence, to simplify this case we follow the SRP principle like this:

```dart
class ComputerStart{
    int config = 0;//default config

    Start(){
        print("Computer Started.");
        config = 1;
    }
}

class ComputerEnd{
    int config = 0;//default config

    End(){
        print("Computer Shut Down.");
        config = 0;
    }
}
```

Our case has been simplified since now each class `ComputerStart` and `ComputerEnd` has a single responsibility. It is easier to implement them and change them in the future.
<br><br>

## 2. Open-Closed Principle (OCP)

Open-closed Principle (OCP) states:

    Objects or entities should be open for extension but closed for modification.

This means that a class/function should be extendable without modifying the class/function itself. This helps us to replicate similar code for different scenarios without modifying the original class/function itself.
<br>

Lets have a look at the following code:

```dart
public long GetCarPrice(String[] models){

    long price = 0;

    foreach (String model in models){
        if(model is "Alto"){
            price = 400000;
        }
        if(model is "Swift"){
            price = 600000;
        }
        if(model is "SX4"){
            price = 900000;
        }
    }
    return price;
}
```

Here we have a function `GetCarPrice` that takes a string array `models` and cycles through each model to get its `price` and return it. However, for each new model that needs to be added to the array, this function has to modified to get the price for that respective model.
<br>

Again, this is a cumberstome process and the code reusability is very low. It violates the OCP principle since the function is open for modification, but not open for extension.
<br>
Here is a better approach:

 ```dart
 public abstract class CarModel{
     public abstract long GetCarPrice();
 }

 public class Alto : CarModel{
     public override long GetCarPrice(){
         return 400000;
     }
 }

 public class Swift : CarModel{
     public override long GetCarPrice(){
         return 600000;
     }
 }

 public class SX4 : CarModel{
     public override long GetCarPrice(){
         return 900000;
     }
 }
 ```

Here we have first created an abstract class `CarModel` that has an abstract function `GetCarPrice`. Now, any existing/new car model can override the `GetCarPrice` function by implementing the `CarModel` class easily.<br>
Therefore, the `CarModel` class is open for extension, but but closed for modification.
<br><br>

## 3. Liskov Substitution Principle (LSP)

Liskov Substitution Principle states:

    Let q(x) be a property provable about objects of x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.

This means that every subclass or derived class should be substitutable for their base or parent class.
<br>
The principle defines that objects of a superclass shall be replaceable with objects of its subclasses without breaking the application. That requires the objects of the subclasses to behave in the same way as the objects of the superclass.
<br>
Let's take an example:

```dart
class Physics {
    Physics(time, marks){
        this.time = time;
        this.marks = marks;
    }

    setTime(){
        time = 60;
        return time;
    }

    getMarks(marks){
        return marks;
    }
}

class Maths extends Physics {

    setTime(){
        time = 70;//different time alloted
        return time;
    }

    getMarks(marks){
        return marks;
    }
}
```

Here we can see superclass `Physics` and its subclass `Maths`. The `Maths` subclass extends its superclass and replaces all of its functions in the same way as used in the superclass. However, in the function `setTime()`, a different time has been alloted. This violates the LSP law since all objects of subclass should behave in the same way as the superclass objects.
<br>
Hence a forceful inheritance has been used to reuse and shorten code, but not in the correct format. A simple solution to this problem is given below:

```dart
class Subject {
    setTime() {
        //default code
    }
}

class Physics extends Subject {
    Physics(time, marks){
        this.time = time;
        this.marks = marks;
    }

    setTime(){
        time = 60;
        return time;
    }

    getMarks(marks){
        return marks;
    }
}

class Maths extends Subject {
    Maths(time, marks){
        this.time = time;
        this.marks = marks;
    }

    setTime(){
        time = 70;//different time alloted
        return time;
    }

    getMarks(marks){
        return marks;
    }
}
```

In the above solution, we have changed the superclass to `Subject`. Both `Physics` and `Maths` classes now inherit from this superclass and have function `setTime` that behaves in the same way as the one in the superclass. LSP is thus followed.
<br><br>

## 4. Interface Segregation Principle (ISP)

Interface segregation principle states:

    A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use.

The dependency of one class to another one should depend on the smallest possible interface. Many interfaces should be created based on groups of methods, each one serving one submodule. This reduces complexity and forceful-dependencies.
<br>
Lets take a look at this code for better understanding:

```dart
class Car {
    startEngine();
    stopEngine();
    sportsMode();
}

class HondaCity implements Car{
    startEngine(){
        //implementation
    };
    stopEngine(){
        //implementation
    };
    sportsMode(){
        //implementation
    };
}

class Alto implements Car{
    startEngine(){
        //implementation
    };
    stopEngine(){
        //implementation
    };
    sportsMode(){
        //Dummy implementation (to avoid errors)
    };
}
```

In the above example, we have a class `Car` that has three abstract methods: `startEngine()`, `stopEngine()` and `sportsMode()`. Both classes `HondaCity` and `Alto` implement the implicit interface declaration for this class. But `Alto` class is forced to implement the `sportsMode()` function/dependency even though the car does not support this feature and does not require it. ISP is therefore violated and this is not a good implementation.
<br>
For a better implementation, we can look at the following code:

```dart
class Car {
    startEngine();
    stopEngine();
}

class SportsFeature extends Car{
    sportsMode();
}

class HondaCity implements SportsFeature{
    startEngine(){
        //implementation
    };
    stopEngine(){
        //implementation
    };
    sportsMode(){
        //implementation
    };
}

class Alto implements Car{
    startEngine(){
        //implementation
    };
    stopEngine(){
        //implementation
    };
}
```

We created an extra class `SportsFeature` that implements the implicit interface declaration for the `Car` class. Now all classes have the option to implement functions/dependencies as per their requirements and are not forced to implement all functions/dependencies like before. This is a proper method of implementation that should be followed as per the ISP.
<br><br>

##  5. Dependency Inversion Principle (DIP)

Dependency inversion principle states:

    Entities must depend on abstractions, not on concretions. It states that the high-level module must not depend on the low-level module, but they should depend on abstractions.

This means that all of our dependencies must have an abstract class or wrapper class around them so that when these dependencies are being used by other classes, they communicate only with the abtraction/wrapper.
<br>
To illustrate this principle, we have an example:

```dart
class School {
    School(studentId) {
        this.attendance = new Attendance(studentId);
    }

    studentAttendance(studentName) {
        this.attendance.markAttendance(studentName);
    }
}

class Attendance {
    Attendance(studentId) {
        this.studentId = studentId;
    }

    markAttendance(studentName) {
        print("Attendance marked for $studentName. ID: ${this.studentId}");
    }
}
```

We have a class `School` that directly depends upon the class `Attendance` to mark students' attendance. This might lead to problems in the future if the `Attendance` class had some changes. We would be required to make changes in the `School` class as well each time any such changes would be made or if the dependency would itself be entirely changed from `Attendance` to some other class.
<br>
To eliminate this problem, we follow DI principle as follows:

```dart
class School {
    School(attendanceWrapper) {
        this.attendanceWrapper = AttendanceWrapper;
    }

    studentAttendance(studentName) {
        this.attendanceWrapper.mark(studentName);
    }
}

class AttendanceWrapper {
    AttendanceWrapper(studentId) {
        this.attendance = new Attendance(studentId);
    }

    mark(studentName) {
        this.attendance.markAttendance(studentName);
    }
}

class Attendance {
    Attendance(studentId) {
        this.studentId = studentId;
    }

    markAttendance(studentName) {
        print("Attendance marked for $studentName. ID: ${this.studentId}");
    }
}
```

Here we see that the `School` class now depends on the abstraction `AttendanceWrapper` and not on the dependency itself, Therefore `School` class will not require to be changed if the `Attendance` class is changed in the future. In other words, we have inverted the inversion from low-level dependency to an abstraction of that dependency, showcasing the DI principle.