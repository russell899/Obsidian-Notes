==access modifier ==
public: Members are accessible from anywhere (outside the class). Constructors are often public so objects can be created, but they can also be protected/private.
private:  Members are accessible only inside the class. It's typically used to hide data and enfore encapsulation
protected: Members are accessible inside the class and its child classes, but nit from outside code.

==Encapsulation==
Encapsulation means bundling data(attributes) and methods into a class, while hiding internal data (typically private) and providing public methods to control access.

==Abstraction==
Abstarction means hiding implementation details and exposing only the essential features through a simple interface


==Inheritance==
Inheritance means a child class has all property of their parent class (function and attribute), and then adds or changes features to fit their needs.

code1:
``` cpp
#include <iostream>
using namespace std;

class Employee{

    private:

        string Name;

        string Company;

        int Age;

  

    public:

        void setName(string name)

        {

            Name = name;

        }

        string getName()

        {

            return Name;

        }

        void setCompany(string company)

        {

            Company = company;

        }

        string getCompany()

        {

            return Company;

        }

        void setAge(int age)

        {

            Age = age;

        }

        int getAge()

        {

            return Age;

        }

        Employee(string name, string company, int age)

        {

            Name = name;

            Company = company;

            Age = age;

        }

        void IntroduceYourself()

        {

            cout << "Name:" << Name << endl;

            cout << "Company:" << Company << endl;

            cout << "Age:" << Age << endl;

        }

};


class Developer: Employee{

    public:

        string FavProgrammingLanguage;

  

        Developer(string name, string company, int age, string favProgrammingLanguge): Employee(name, company, age)

        {

            FavProgrammingLanguage = favProgrammingLanguge;

        }

        void FixBug()

        {

            cout << getName() << " fixed bug using " << FavProgrammingLanguage << endl;

        }

};
  

int main()

{

    Developer d = Developer("Shihao","Tesla",25,"C++");

    //d.IntroduceYourself();

    d.FixBug();

}
```

so for the above example, since we did not define which kind of the inheritence type it is, so the default inheirtenance type is private, and for the private class, child object could not access  parents function, （eg. d.IntroduceYourself())

In this example, you still need to create constructor on your own, although when you create a class, there will be a default constructor. However, in this example, since develop class is inheriting from Employee, and there have already a constructor on Employee class, so you need to create a new constructor again by ourselves.

`Developer(string name, string company, int age, string favProgrammingLanguge): Employee(name, company, age)` This is how constructor function inherit from parents constructor function.

``` cpp
class Developer: public Employee{

    public:

        string FavProgrammingLanguage;

  

        Developer(string name, string company, int age, string favProgrammingLanguge): Employee(name, company, age)

        {

            FavProgrammingLanguage = favProgrammingLanguge;

        }

        void FixBug()

        {

            cout << Name << " fixed bug using " << FavProgrammingLanguage << endl;

        }

};

int main()

{

    Developer d = Developer("Shihao","Tesla",25,"C++");

    d.IntroduceYourself();

    d.FixBug();

}

```

once we define the inheritence type as public, the object from child class can access IntroduceYourself() function. And also, we define 'Name' in protected area, this attribute can also be accessed by child class.

==Ploymorphism==
Ploymorphism means 'one interface, many implementations': calling the same method on different derived objects can produce different results.

code:
```cpp
#include <iostream>

using namespace std; 

class Employee{

    protected:

    string Name;

    private:

        //string Name;

        string Company;

        int Age;

  

    public:

        void setName(string name)

        {

            Name = name;

        }

        string getName()

        {

            return Name;

        }

        void setCompany(string company)

        {

            Company = company;

        }

        string getCompany()

        {

            return Company;

        }

        void setAge(int age)

        {

            Age = age;

        }

        int getAge()

        {

            return Age;

        }

        Employee(string name, string company, int age)

        {

            Name = name;

            Company = company;

            Age = age;

        }

        void IntroduceYourself()

        {

            cout << "Name:" << Name << endl;

            cout << "Company:" << Company << endl;

            cout << "Age:" << Age << endl;

        }

  

        void work()

        {

            cout << Name << " is working..." << endl;

        }

};

  

class Developer: public Employee{

    public:

        string FavProgrammingLanguage;

  

        Developer(string name, string company, int age, string favProgrammingLanguge): Employee(name, company, age)

        {

            FavProgrammingLanguage = favProgrammingLanguge;

        }

        void FixBug()

        {

            cout << Name << " fixed bug using " << FavProgrammingLanguage << endl;

        }

        void work()

        {

            cout << Name << " is writing " << FavProgrammingLanguage << " code..." << endl;

        }

};

  
int main()

{

    Employee e = Employee("Shihao","Tesla",25);

    Developer d = Developer("Shihao","Tesla",25,"C++");

    e.work();

    d.work();

}
```

In this example, since Employee e and Developer d are object from different class, and in the child class and parents class, they both define work() function, and they content of this function is not the same, so when these two objects call them, the results are not the same.

==Parents class pointer ==

code:
```cpp
int main()

{

    Employee e = Employee("Shihao","Tesla",25);

    Developer d = Developer("Shihao","Tesla",25,"C++");

    Employee *a = &e;

    Employee *b = &d;

    a->work();

    b->work();

}
```
When we use a base-class pointer (or reference), it stores the address of an object, and we call methods like `ptr->func()`.  
If `work()` is **not virtual**, the call is resolved at compile time, so it will call the **base-class** version (static binding).  
If `work()` is declared **virtual** in the base class and overridden in the derived class, the call is resolved at runtime, so it will call the **derived-class** version (dynamic binding / polymorphism).

==virtual function==

A virtual function allows a derived class to override a base-class function,, so that calling the function through a base-class pointer/reference will execute the derived version. (Declear virtual function in parents class function instead of child class function)

`Interview follow-up: Why should the destructor be virtual?`
If you `delete` a derived object through a base-class pointer, and the base destructor is **not**virtual, only the base destructor may run, so the derived part won’t be destroyed properly and resources can leak.

``` cpp
#include <iostream>
using namespace std;


class Employee{

    protected:

    string Name;

    private:

        //string Name;

        string Company;

        int Age;

  

    public:

        void setName(string name)

        {

            Name = name;

        }

        string getName()

        {

            return Name;

        }

        void setCompany(string company)

        {

            Company = company;

        }

        string getCompany()

        {

            return Company;

        }

        void setAge(int age)

        {

            Age = age;

        }

        int getAge()

        {

            return Age;

        }

        Employee(string name, string company, int age)

        {

            Name = name;

            Company = company;

            Age = age;

        }

        void IntroduceYourself()

        {

            cout << "Name:" << Name << endl;

            cout << "Company:" << Company << endl;

            cout << "Age:" << Age << endl;

        }

  

        virtual void work()

        {

            cout << Name << " is working..." << endl;

        }

};

  
class Developer: public Employee{

    public:

        string FavProgrammingLanguage;

  

        Developer(string name, string company, int age, string favProgrammingLanguge): Employee(name, company, age)

        {

            FavProgrammingLanguage = favProgrammingLanguge;

        }

        void FixBug()

        {

            cout << Name << " fixed bug using " << FavProgrammingLanguage << endl;

        }

        void work()

        {

            cout << Name << " is writing " << FavProgrammingLanguage << " code..." << endl;

        }

};


int main()

{

    Employee e = Employee("Shihao","Tesla",25);

    Developer d = Developer("Shihao","Tesla",25,"C++");

    Employee *a = &e;

    Employee *b = &d;

    a->work();

    b->work();

}
```
It will output different result.