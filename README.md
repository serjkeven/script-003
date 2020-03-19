# script-003
import sys
import gc
import functools as ft
import random as rn

#uakazanie na chisla kak na object
a = 1
b = a
print(sys.getrefcount(a))

#ochistka peremennih
class A:
    def __del__(self):
        print("Del")
a = A()
a = 1

#garbage collector
l = [1,2,3]
l.append(l)
print(l)
l = 1 
gc.collect()

#vieboni s funciuami
def double(x):
    return 2*x
f = double
print(f(3))

def execute(f,x):
    return f(x)
print(execute(double,2))

def f1():
    def g(x):
        return 2*x
    return g
print(f1())
double = f1()
print(double(2),f1()(2))

#mapping - prmienenie funciy k konteineru
a = map(double,[1,2,3])  #map bolshe dlya funcii
b = list(map(double,[1,2,3]))
c = [2*x for x in [1,2,3]] #chashe ispolzovat list compehansions
d = [2*x for x in (1,2,3)] 
e = (2*x for x in [1,2,3]) #vipolnyaet menshe coda
print(list(e))
f = list(map(str,range(1,10)))

#filtering mazafucka
def odd(x):
    return x%2
g = list(filter(odd,[1,2,3,4]))    #filter eto kak map tolko dlya true false
h = [x for x in [1,2,3,4] if x%2]  #toje samoe chto list compahansions

#reduce probyem
def add(x,y):
    return(x+y)

j = ft.reduce(add,[1,2,3,4])
k = ft.reduce(add,["a","b","c","d"])
l = ft.reduce(add,map(str,range(10)))

def mul(x,y):
    return x*y

m = ft.reduce(mul,range(1,10))  #high order function


#lamda functions
n = ft.reduce(lambda x,y : x + y, [1,2,3,4]) #lamda sintaksis dlya ananimnih funcii

# comparison
o = (1,2) < (2,1)
p = (1,2) < (1,1)

q = [(2,1),(1,4),(3,2),(1,3),(2,2)]
r = sorted(q)  # eto shtob novii spisok sozdat'
q.sort() #копии копируют только первый уровень, а второй - копируются ссылки - дальше только костялть
         #пробуйте copy.deepcopy()
print(r)
r.sort(key=lambda x: x[1])
rn.shuffle(r)
print(r)
r.sort(key=lambda x: x[1]) 
r.sort(key=lambda x: x[0] - x[1])  #podchet rasstoyaniya mejdu vectorami
s = min(r, key = lambda x: x[0] + x[1])


#zamikaniya kakie to ??? chto ?? zachem
#из внешнего контекста захватывается некое значение и замыкается внутри функции
t = []
for i in range(10):
    def a1(x,i=i):
        return x*i
    t.append(a1)
t = t[5](0)


#pohoje na dekorator kakoi to
#cozdanie funkciy
def u(x):    #фабрика функци - по умножению то на два, то на три
    def v(y):
        return x*y
    return v

double = u(2)
tripple = u(3)
print(double(2),tripple(2))

#декорируем что то - ебать сложно
def get_area(x):
    return x*x

def scale(f):
    def wrapper(x):
        x *= 100
        res = f(x) 
        # ---- 
        return res
    return wrapper

get_area1 = scale(get_area)
print(get_area1(5))

@scale
def get_area(x):
    return x*x

def log(f):
    def wrapper(*arg,**kwarg):   #* - упаковка при обьявлении
        print(f.__name__, "<-", arg, kwarg)
        res = f(*arg, **kwarg)  #* - распаковка при вызове
        print(f.__name__, "->", res)
    return wrapper

@log
def add(x,y):
    return x + y

add(2,3)
add(3, y=5)

@log
@scale
def get_area(x):
    return x*x

get_area(5)


def scale(k):
    def decorator(f):
        def wrapper(x):
            return f(x*k)
        return wrapper
    return decorator


@log
@scale(20)
def get_area(x):
    """Function f"""
    return x*x

get_area(5)


def scale(f):
    @ft.wraps(f)
    def wrapper(x):
        return f(x*k)
    return wrapper

@scale
def get_area(x):
    """Function f"""
    return x*x

print(get_area)
print(help(get_area))

