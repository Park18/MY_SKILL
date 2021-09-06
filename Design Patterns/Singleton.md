# Singleton(단일체) 패턴

> 오직 한 개의 클래스 인스턴스만을 갖도록 보장하고, 이에 대한 전역적인 접근점을 제공합니다. (GoF의 디자인 패턴 p.181)

## 구현
### Singleton.hpp
```c++
class Singleton
{
public:
    /*!
     * @brief   Singleton 클래스의 인스턴스를 반환하는 메서드(생성자)
     * @return  Singleton 클래스의 인스턴스 포인터
     */
    static Singleton* Instance();

protected:
    /*!
     * @brief   Singleton 클래스의 생성자
     * @details C++에서 기본으로 지원하는 생성자로, 인스턴스를 요청하면 무조건 생성해준다.
                하지만 Singleton 패턴에서는 오직 한개의 클래스 인스턴스만 갖도록 보장해야하기 때문에
                "protected"나 "private" 키워드로 인스턴스를 생성할 수 없게 지정한다.
     */
    Singleton();
private:
    static Singleton* _instance;
}
```
### Singleton.cpp
```c++
Singleton* Singleton::_instance = 0;

Singleton* Singleton::Instance()
{
    if(_instance == 0)
    {
        _instance = new Singleton;
    }

    return _instance;
}
```