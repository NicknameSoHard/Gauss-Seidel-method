#include <iostream>
#include <fstream>
#include <cmath>
using namespace std;
 
bool Slau(int n, double e, double** const a, double* const b, double *x)
{
    if(e<=0)
        return false;
 
    double sum=0;
    for(int i=0;i<n;i++) // Проверка диагональной доминанты и на ошибки матрицы
    {
        sum=0;
        for(int j=0;j<n;j++)
            if(i!=j)
                sum+=fabs(a[i][j]);
        if(fabs(a[i][i]) < sum)
            return false;
    }
 
    double** matrix= new double*[n]; // Копия матрицы
    for(int i=0;i<n;i++)
    {
        matrix[i]=new double[n];
        for(int j=0;j<n;j++)
            matrix[i][j]=a[i][j];
        x[i]=0; //Дополнительно обнуляем существующее решение
    }
 
    double* pastDec= new double[n]; //Создаем массив для череды прошлых решений
    bool end;
    do
    {
        end=true;
        for (int i = 0; i < n; i++) // Для корректировования результата записываем предыдущее решение
            pastDec[i] = x[i];
 
        for(int i=0; i<n;i++)
        {
            x[i]=b[i];
            for(int j=0;j<n;j++)
            {
            if(i<j)
                x[i] -= matrix[i][j]*x[j];
            if(i>j)
                x[i] -= matrix[i][j]*pastDec[j];
            }
            x[i]/=matrix[i][i];
        }
 
        for(int i=0;i<n;i++)
            if(fabs(x[i]-pastDec[i]) > e)
            {
                end=false;
                break;
            }
    }while (!end);
 
    delete[] pastDec;
    for(int i=0;i<n;i++) // очищаем память
        delete[] matrix[i];
    delete[] matrix;
    return true;
}
 
bool ShowMatrix(int n, double** const a, double* const b)
{
    for(int i=0; i<n;i++)
    {
        for(int j=0; j<n;j++)
        {
            cout << a[i][j] << "*x" << j+1;
            if (j+1<n)
                if(a[i][j+1] > 0)
                    cout << " + ";
                else
                    cout << " ";
        }
        cout << "=" << b[i] << endl;
    }
    cout << endl;
    return true;
}
 
bool OnConsole(int n, double* a)
{
    for(int i=0;i<n;i++)
        cout <<"x"<<i+1<<"=" << a[i] << " ";
    cout << endl;
}
 
bool CheckResult(int n, double** a, double* x)
{
    double sum;
    for(int i=0;i<n;i++)
    {
        sum=0;
        for(int j=0;j<n;j++)
                sum+=a[i][j]*x[j];
 
        for(int j=0;j<n;j++)
        {
            cout<<a[i][j]<<" * "<<x[j];
            if( j+1!=n && a[i][j+1] > 0)
                cout << " +";
            else
                cout << " ";
        }
        cout<< "= "<<sum<< endl;
    }
    return true;
}
 
int main()
{
    char choise;
    cout<< "Файл 1 или 2? ";
    cin>> choise;
 
    char* file;
    if(choise=='1')
        file= "SLAU Matrix Doc 1";
    else
        file= "SLAU Matrix Doc 2";
 
    ifstream in(file); // Считываем матрицу
    if(in)
    {
        int n=0;
        in >> n;
        if(n<=0)
        {
            cout<<"Ошибка чтения матрицы. Неверный формат размера"<<endl;
            break;
        }
        double** a= new double* [n]; // Заданная матрица
        double* b= new double [n]; // Массив правых частей
        for(int i=0, bc=0;i<n;i++)
        {
            a[i]=new double[n];
            for(int j=0;j<n;j++)
            {
                in >> a[i][j];
                if (j+1==n)
                {
                    in >> b[bc];
                    bc++;
                }
            }
        }
        in.close();
 
        double* x=new double[n]; // Массив решений
        ShowMatrix(n,a,b);
 
        double e;
        cout << "Введите ожидаемую точность: ";
        cin >> e;
 
        if(!Slau(n,e,a,b,x))
            cout<< "Решение системы остановлено. Для данной матрицы метод Зейделя неэффективен."<< endl;
        else
        {
            OnConsole(n,x);
            cout<<endl<<"Проверка результата:"<<endl;
            CheckResult(n,a,x);
        }
 
        for(int j=0;j<n;j++)
            delete[] a[j];
        delete[] a;
        delete[] b;
        delete[] x;
    }
    else
        cout<<"Файлы с матрицами не найдены." << endl << "Пожалуйста, создайте файл ""SLAU Matrix Doc"" в корневой папке."<< endl;
 
    return app.exec();
}
