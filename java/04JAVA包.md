# 04JAVA包

用于防止命名冲突、访问控制等

```java
package pkg1[.pkg2[.pkg3...]];
```

## 创建包

- 用小写命名

`Animal.java`

```java
package animals;

interface Animals {
	public void eat();
	public void travel();
}
```

`MammalInt.java`

```java
package animals;
 
/* 文件名 : MammalInt.java */
public class MammalInt implements Animal{
 
   public void eat(){
      System.out.println("Mammal eats");
   }
 
   public void travel(){
      System.out.println("Mammal travels");
   } 
 
   public int noOfLegs(){
      return 0;
   }
 
   public static void main(String args[]){
      MammalInt m = new MammalInt();
      m.eat();
      m.travel();
   }
}
```

编译两个文件，把他们放在animals的子目录中

```
$ mkdir animals
$ cp Animal.class  MammalInt.class animals
$ java animals/MammalInt
Mammal eats
Mammal travel
```

## import 

```
import package1[.package2…].(classname|*);
```

- 如果在一个包中，一个类想要使用本包中的另一个类，那么该包名可以省略。
- 类文件中可以包含任意数量的 import 声明。import 声明必须在包声明之后，类声明之前。