```c++
/*test_io.h*/

#include <ios>
#include <iostream>
#include <fstream>
#include <cstdio>

#include "gtest/gtest.h"

using namespace std;

typedef enum
{
    IO_TYPE_CIN,
    IO_TYPE_COUT,
    IO_TYPE_MAX
}IO_TYPE_ENUM;

//测试I/O的test_fixture
class TEST_IO : public testing::Test
{
public:
    TEST_IO():test_case_path("..\\..\\..\\testcase\\")
    {
        cin_backup = NULL;
        cout_backup = NULL;

        cstdio_in_redirect_flag = false;
        cstdio_out_redirect_flag = false;
    }

    ~TEST_IO() 
    {    
        io_stream_revert();  
        cstdio_revert();
    }
    
    void io_stream_redirect(IO_TYPE_ENUM io_type, const char* file_name);

    void cstdio_redirect(FILE* stream, const char* file_name);

    virtual void SetUp();
    virtual void TearDown();

private:
    void io_stream_revert();
    void cstdio_revert();

    const string test_case_path;

    bool cstdio_in_redirect_flag, cstdio_out_redirect_flag;

    fstream fin, fout;
    streambuf *cin_backup, *cout_backup;
};



/*test_io.cpp*/
#include <string>

#include "test_io.h"

void TEST_IO::io_stream_redirect(IO_TYPE_ENUM io_type, const char* file_name)
{
    string redirect_file = test_case_path + file_name;

    if (io_type == IO_TYPE_CIN)
    {
        cin_backup = cin.rdbuf();
        fin.open(redirect_file);
        cin.rdbuf(fin.rdbuf());
    }

    if (io_type == IO_TYPE_COUT)
    {
        cout_backup = cout.rdbuf();
        fout.open(redirect_file);
        cout.rdbuf(fout.rdbuf());
    }
}

void TEST_IO::io_stream_revert()
{
    if (cin_backup != NULL)
    {
        cin.rdbuf(cin_backup);
        fin.close();
    }

    if (cout_backup != NULL)
    {
        cout.rdbuf(cout_backup);
        fout.close();
    }
}

void TEST_IO::cstdio_redirect(FILE* stream, const char* file_name)
{
    string redirect_file = test_case_path + file_name;

    if (stream == stdin)
    {
        freopen(redirect_file.c_str(), "r", stdin);
        cstdio_in_redirect_flag = true;
    }

    if (stream == stdout)
    {
        freopen(redirect_file.c_str(), "w", stdout);
        cstdio_out_redirect_flag = true;
    }
}

void TEST_IO::cstdio_revert()
{
    if (cstdio_in_redirect_flag)
    {
        fclose(stdin);
        freopen("CON", "r", stdin);
    }

    if (cstdio_out_redirect_flag)
    {
        fclose(stdout);
        freopen("CON", "w", stdout);
    }
}
```
