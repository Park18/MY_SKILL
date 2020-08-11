# 1. boost::filesystem
boost::filesystem 라이버리는 파일, 디렉토리에 대한 작업을 단순화 시켜준다. 라이브러리는 파일 경로를 의미하는 boost::filesystem::path 라는 클래스를 제공한다. 또, 많은 자유 함수(비멤버 함수)를 이용해 디렉토리를 만들거나 파일이 존재하는지 검사하는 등의 일을 할 수 있다. (boost.filesysetm 은 여러번 개편되었는데 버전 2와 버전 3이 존재한다. 1.46 버전부터 filesystem 버전 3이 기본 버전으로 포함되어 릴리즈 되었다.)

## 1.1. path
boost::filesystem::path 은 boost.filesystem 에서 처리 중인 파일 경로를 표현하기 위한 클래스이다. 이 클래스에 대한 정의는 boost/filesystem.hpp 안의  boost/filesystem/path.hpp 에서 찾을 수 있으며, boost::filesystem::path의 생성자로 문자열을 넘김으로써 경로를 만들 수 있다.

```c++
#include <boost/filesystem.hpp>

using namespace boost::filesystem;

int main()
{
    bfs::path p1{"C:\\"}
    bfs::path p2{"C:\\Windows"};
    bfs::path p3{"C:\\Boost C++"};
}
```

boost::filesystem::path 는 와이드 문자열을 통해서도 초기화 될 수 있다. 와이드 문자열은 유니코드로 해석되고 거의 모든 언어를 사용해 쉽게 경로를 만들 수 있다. 이 부분은 버전 2와 큰 차이점 중 하나인데, 기존에는 boost::filesystem::path과 boost::filesystem::wpath를 제공하였으며, 문자열 타입에 맞춰서 사용해야 했다.

주의해야 할 점은 boost.filesystem 은 std::u16string 혹은 std::u32string 을 지원하지 않는다는 점이다. 만약 이러한 타입을 사용한다면 boost::filesystem::path을 생성할 때 컴파일 에러를 발생시킬 것이다.

boost::filesystem::path 의 생성자들은 경로의 유효성을 검사하거나 지정된 파일 혹은 디렉토리가 있는지 여부를 확인하지 않는다. 따라서 path 은 무의미한 경로로 인스턴스화 될 수 이싸.

boost::filesystem::path 는 문자열만을 처리하며 파일 시스템에 접근하지 않는다.

```c++
#include <boost/filesystem.hpp>

namespace bfs = boost::filesystem;

int main()
{
    bfs::path p1{".."}
    bfs::path p2{"\\"};
    bfs::path p3{"@:"};
}
```

위 코드는 어떠한 문제도 발생기키지 않는다. 단순한 문자열이기 때문이다.

boost.filesystem 의 경로에는 native path 와 generic path 라는 것이 있다. native path 는 호출한 운영체제에 종속적이며, generic path 는 운영체제에 종속되지 않은 portable한 경로이다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{"C:\\Windows\\System"};

#ifdef BOOST_WINDOWS_API
    wcout << p.native() << endl;
#else
    cout << p.native() << endl;
#endif
    cout << p.string() << endl;
    wcout << p.wstring() << endl;
    cout << p.generic_string() << endl;
    wcout << p.generic_wstring() << endl;
}
```

native(), string(), wstring() 멤버 함수들은 모두 native 포멧을 반환하며 generic_string 과 generic_wstring() 은 generic 포멧을 반환한다. 여기서 generic 포멧이란 POSIX 표준에 의거하여 정규화된 문자열이다. 그러므로 generic path 은 리눅스에도 사용할 수 있다. 예를 들어 Windows 에서 generic_string() 과 generic_wstring() 을 콘솔에 출력하면 "C:/Windows/System" 이 출력 될 것이다.

native path 의 반환 값은 실행중인 운영체제에 따라 다르지만 generic path 의 반환 값은 운영체제와는 별개이기 때문에 generic paht 는 운영 체제와 독립적으로 파일 및 디렉토리를 고유하게 식별하므로 플랫폼 독립적 코드를 쉽게 작성할 수 있다.

string() 및 generic_string() 은 std::string 타입을 반혼하지만 wstring() 및 generic_wstring() 은 std::wstring 타입을 반환한다.

native() 는 컴파일한 운영체제에 따라 다르다. Windows 에서는 std::wstring타입이 반환되지만 Linux 에서는 std::string 타입이 반환된다.

boost::filesystem:path 의 생성자는 generic 경로와 플랫폼 종속 경로를 모두 지원하는데, "C:\\Windows\\System" 경로는 Windows 전용이며 portable 하지 않다. 또한 "\"는 C++ 에서 에스케이프 문자이므로 "\\" 로 처리해야 한다. 이 경로는 Windows 에서 실행되는 경우에만 올바르게 쓰일 수 있으며, Linux 와 같은 POSIX 호환 운영체제에서 실행되면 "C:\Windows\System" 을 반환한다. Linux 에서는 "\" 는 구분 기호로 사용되지 않으므로 boost.filesystem 은 이 문자를 파일 및 디렉토리 구분 기호로 인식하지 않는다.

다음은 portable 한 경로로 boost::filesystem::path 를 생성한 코드이다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{"/"};

    cout << p.string() << endl;
    cout << p.generic_string() << endl;
}
```

generic_string() 은 portable 한 경로를 반환하기 떄문에 "/"를 반환할 것이며 string() 은 플랫폼에 따라 다른 값을 반환할 것이다. 하지만 Windows 에서는 "/"는 디렉토리 구분 기호로 허용되기 때문에 출력이 "/"로 동일하다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{"C:\\Windows\\System"};

    cout << p.root_name() << endl;
    cout << p.root_directory() << endl;
    cout << p.root_path() << endl;
    cout << p.relative_path() << endl;
    cout << p.parent_path() << endl;
    cout << p.filename() << endl;
}
```

위 코드에서 "C:\\Windows\\System" 문자열은 플랫폼 마다 다르게 해석되는데 Windows의 경우 각각

root_name():        "C:"
root_directory():   "\"
root_path():        "C:\"
relative_path():    "Windows\System"
parent_path():      "C:\Windows"
filename():         "System"

를 반환한다.

이 경로를 portalbe 한 포맷으로 표기하기 위해서는 generic_string()을 명시적으로 호출해야 한다.

만약 위 코드를 Linex 에서 실행하면 relative_path() 와 filename() 을 제외한 함수들이 빈 문자열을 반환할 것이다. 반면 위 relative_path() 와 filename() 은 "C:\Windows\System"을 반환하는데, "C:\\Windows\\System"은 Linux 에서 파일 이름으로 통째로 해석되기 떄문이다.

boost.filesystem 은 경로에 특정 하위 문자열이 들어있는지 확인하기 위해 다양한 멤버 함수를 제공한다.

1. has_root_name()
2. has_root_directory()
3. has_root_path()
4. has_filename()

함수를 통해 이를 알 수 있으며 bool 타입을 반환한다.

has_filename() 이 true 를 반환할 경우에만 사용할 수 있는 두개의 추가 함수도 있다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{"photo.jpg"};

    cout << p.stem() << endl;
    cout << p.extension() << endl;
}
```

p.stem():       "photo"
p.extension():  ".jpg"

를 반환한다.

> boost::filesystem::path 은 실제로 존재하는 파일인지 확인하지 않기 떄무에 파일이나 디렉토리가 존재하지 않아도 has_filename() 은 true 를 반환한다.

멤버 함수 호출을 통해 구성 요소에 접근하는 대신 iterator 를 사용할 수 있다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{"C:\\Windows\\System"};

    for(const bfs::path &pp : p)
        cout << pp << endl;
}
```

Windows 의 경우 "C:", "/", "Windows", "System"을 반환하지만 Linux 의 경우 "C:\Windws\System"을 반환한다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{"C:\\Windows\\System"};

    p /= "photo.jpg";
    cout << p.string() << endl;
}
```

operator/= 를 사용하면 경로를 추가할 수 있다. Windows에서 실행하면 "C:\Windows\System\photo.jpg"가 출력되지만, Linux에서는 "C:\Windows\System\/photo.jpg"가 출력된다.

operator+= 과 다른 점은 디렉토리 구분자가 붙는다는 것이다. operator+=를 사용하면 "C:\Windows\Systemphoto.jpg"가 출력된다.

이 외에 remove_filename(), replace_extension(), make_absolute(), make_preferred() 같이 경로를 수정하기 위한 다양한 함수가 존재한다. 마지막 함수의 경우 특히 Windows 에서 사용되도록 설계되었다.

> * remove_filename():  filename()에 해당하는 문자열을 삭제한다.
> * replace_extension:  extension()에 해당하는 문자열을 삭제한다.
> * make_absolute():    boost 1.71.1 버전에선 안뜬다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{"C:\\Windows\\System"};
    cout << p.make_preferred() << endl;
}
```

백 슬래시는 파일 및 디렉토리의 구분기호로 사용되고 Windows 에서도 역시 이를 허용한다. 하지만 make_preferred() 를 사용하면 Windows 의 기본 표기법으로 변환할 수 있다. 즉, "C:\Windows\System" 을 출력한다.

make_preferred() 는 Linux 와 같은 POSIX 호환 운영체제에는 영향을 미치지 않는다. make_preferred() 함수는 변환된 경로를 반환할 뿐만 아니라 객체를 수정하므로 주의해야 한다.

## 1.2. Files and Directories
boost::filesystem::path는 단순히 문자열을 처리한다. 경로의 개별 구성 요소에 접근하고, 수정하는 등의 작업을 수행한다.

하드 드라이브의 실제 파일과 디렉토리로 작업을 하기 위해서 몇 가지 자유 함수들을 제공한다. 이 함수들은 하나 이상의 boost::filesystem:;path 매개변수를 넘겨받고, 내부적으로 운영체제 시스템 함수들을 호출한다.

이런 다양한 기능들을 소개하기에 앞서, 오류가 발생할 경우 어떻게 해야하는지를 알아야한다. 모든 함수들은 실패가 가능한 운영체제의 시스템 함수를 호출하게 된다. 때무에 boost.filesystem은 오류가 발생했을 때를 처리하기 위한 두 가지 방법을 제공한다.

1. boost::filesystem::filesystem_error 타입의 예외를 던진다.
2. boost::filesystem::error_code 객체를 함수의 참조 매개변수로 전달하여, 함수 호출 뒤 이를 검사하여 처리한다.

boost::filesystem::filesystem_error는 boost::filesystem::path를 반환하는 path1(), path2() 두개의 멤버 함수를 제공한다. 이 멤버 함수들은 오류 발생시 해당 경로를 쉽게 찾을 수 있도록 해준다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{ "C:\\" };

    try
    {
        bfs::file_status s = bfs::status(p);
        cout << boolalpha << bfs::is_directory(s) << endl;
    }
    catch (bfs::filesystem_error& e)
    {
        cerr << e.what() << endl;
    }
}
```

> std::boolalpha란?
> bool a = false, bool b = true 일때
> cout << a << "/" << b << endl; 의 결과로 "0/1"이 출력된다.
> 이를 "false/true"로 출력하기 위해 if/else 를 쓰는 대신 boolalpha를 사용하면
> cout << boolalpha << a << "/" << boolalpha << b << endl; 의 결과로 "false/true"가 출력된다.

위 코드는 파일이나 디렉토리의 상태를 질의하는 boost::filesystem::status()를 보여준다. 이 함수는 boost::filesystem::file_status 타입의 객체를 반환하는데, 나중에 이 객체는 다른 함수를 사용할 때 전달될 수 있다.

예를 들어, boost::filesystem::is_directory() 함수는 디렉토리의 상태가 올바를 때 true를 반환하는데 이때 매개변수로 앞선 객체를 전달한다. 이 외에도 boost::filesystem_is_regular_file(), boost::filesystem::is_symlink(), boost::filesystem::exits() 등의 다른 함수에 사용할 수 있는데, 이 함수들은 bool 타입을 반환한다.

boost::filesystem::symlink_status() 함수는 symbolic link의 상태를 질의한다. boost::filesystem_status()를 사용하면 symbolic link가 참조하는 파일의 상태가 질의된다. Windows에서 symbolic link는 .lnk 확장자로 구분한다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{ "C:\\Windows\\win.ini" };
    boost::system::error_code ec;
    boost::uintmax_t file_size = bfs::file_size(p, ec);

    if (!ec)
        cout << file_size << endl;
    else
        cout << ec << endl;
}
```

자유 함수들의 다른 범주로써 속성 질의가 있다. boost::filesystem::file_size() 함수는 파일의 크기를 boost::uintmax_t라는 타입의 바이트 단위로 반환한다. 이는 unsigned long long을 의미한다. boost::system::error_code 객체를 사용하여 boost::filesystem::file_size() 함수의 호출 성공 여부 또한 확인한다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>
#include <ctime>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{ "C:\\Windows\\win.ini" };

    try
    {
        time_t t = bfs::last_write_time(p);
        cout << ctime(&t) << endl;
    }
    catch (bfs::filesystem_error &e)
    {
        cerr << e.what() << endl;
    }
}
```

파일의 마지막 수정 시간을 확인하기 위해서는 boost::filesystem::last_write_time() 함수를 사용하면 된다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{ "C:\\" };

	try
	{
		bfs::space_info s = bfs::space(p);
		cout << s.capacity << endl;
		cout << s.free << endl;
		cout << s.available << endl;
	}
	catch (bfs::filesystem_error &e)
	{
		cerr << e.what() << endl;
	}
}
```

boost::filesystem::space()는 총 디스크 공간과 나머지 디스크 공간을 구해서 전달한다. 이 함수는 boost::filesystem::space_info 타입의 객체를 반환하는데 이 객체의 public 멤버 변수들(capacity, free, available)을 통해 정보를 확인할 수 있다.

> * capacity:   전체 용량
> * free:       전체 남은 용량
> * available:  예약된 파일 시스템 블록(root 사용자용)

앞서 소개된 함수들은 파일과 디렉토리를 손대지 않은 상태로 유지하지만 파일 및 디렉토리를 생성하고 이름을 변경 또는 삭제하는데 사용할 수 있는 몇 가지 함수들도 있다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
    bfs::path p{ "C:\\Test" };

	try
	{
		if (bfs::create_directory(p))
		{
			bfs::rename(p, "C:\\Test2");
			bfs::remove("C:\\Test2");
		}
	}
	catch (bfs::filesystem_error &e)
	{
		cerr << e.what() << endl;
	}
}
```

위 예제를 보면 함수에 전달되는 경로 값이 boost::filesystem::path이 아닌 간단한 문자열임을 알 수 있는데, 이는 boost::filesystem::path이 문자열을 자신의 타입으로 암묵적으로 변환하는 생성자를 가지고 있기 때문이다. 이를 통해 boost.filesystem을 좀 더 편하게 사용할 수 있다.

위 예제에서 boost::filesystem::remove() 함수는 명시적으로 namespace를 사용하여 호출했는데 이는 stdio.h의 remove() 함수와 혼동이 올 수 있기 때문이다.

symbolic link를 생성하는 create_symlink() 함수도 있으며, 파일 및 디렉토리를 복사하는 copy_file(), copy_directory() 함수도 있다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
	try
	{
		cout << bfs::absolute("photo.jpg") << endl;
	}
	catch (bfs::filesystem_error &e)
	{
		cerr << e.code() << endl;
	}
}
```

위 예제는 파일 이름이나 경로를 기반으로 절대 경로를 만든다. 표시되는 경로는 프로그램이 시작되는 디렉토리에 따라 달라진다. 예를 들어, 프로그램이 "C:\"에서 시작된 경우 "C:\photo.jpg"가 출력된다.

다른 디렉토리를 기죽으로 전대 경로를 표시하려면 두 번째 매개변수로 이를 전달하면 된다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
	try
	{
		cout << bfs::absolute("photo.jpg", "D:\\") << endl;
	}
	catch (bfs::filesystem_error &e)
	{
		cerr << e.code() << endl;
	}
}
```

위 예제는 "D:\photo.jpg"를 출력한다.

마지막 예제는 현재 작업 디렉토리를 나타내는 유용한 헬퍼 함수이다.

```c++
#include <boost/filesystem.hpp>
#include <iostream>

using namespace std;
namespace bfs = boost::filesystem;

int main()
{
	try
	{
		cout << bfs::current_path() << endl;
		bfs::current_path("C:\\");
		cout << bfs::current_path() << endl;
	}
	catch (bfs::filesystem_error &e)
	{
		cerr << e.code() << endl;
	}
}
```

위 예제는 boost::filesystem::current_path()를 여러번 호출하는데, 매개변수 없이 호출되면 현재 작업 디렉토리를 반환한다. 혹은 boost::filesystem::path 타입을 매개변수로 전달하면 현재 작업 디렉토리가 설정된다.

# 참고자료
* [자료1](https://m.blog.naver.com/PostView.nhn?blogId=wizinfantry&logNo=80187751970&proxyReferer=https:%2F%2Fwww.google.com%2F)
* 
* 