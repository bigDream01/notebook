```Java
package boy;

public class boy {

    private String name;
    private  int age;

    public void setName(String name){

        this.name = name;
    }
    public String  getName(){

        return name;
    }
    public void setAge(int age){

        this.age = age;
    }
    public int getAge(){
        return age;
    }

    public  void  marry(Girl girl){
    System.out.println("想娶");
    }

    public void shout(){

    }


}
 class Girl{

    private String name;
    private int age;

    public void setName(String name){

        this.name = name;
    }

    public String getName(){

        return name;
    }
    public void  marry(boy Boy){

        System.out.println("我想嫁给" + );

    }
    public void  compare(Girl girl){

    }
}
```