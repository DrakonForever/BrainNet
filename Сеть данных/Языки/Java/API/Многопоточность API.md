 ## Concurrency API
tags: #java

---

### Термины:

==Атомарная операция== - это операция, которая завершается в один шаг относительно других потоков, имеющих доступ к этой памяти. Во время выполнения такой операции над переменной, ни один поток не может наблюдать изменение наполовину завершенным. Атомарная загрузка гарантирует, что переменная будет загружена целиком в один момент времени. Неатомарные операции не дают такой гарантии. 

==Квант== - небольшой кусочек процессорного времени, который выделяется каждой нити. Когда это время истекает – процессор переключается на другую нить и начинает выполнять ее команды. 
# Работа с потоками
### Создание потока
Создать поток можно двумя способами:
1. C помощью [[class Thread]], в конструктор которого нужно передать класс, реализующий [[$- interface Runnable]].
	```java 
	Thread t = new Thread(new Runnable() {
		@Override
		public void run() {
		//Метод, выполняющийся после запуска потока
		} 
	}
	t.start(); #запуск потока
	```

	Вместе с потоком должна быть задача, которую он выполняет, иначе поток умрет. Поток выполняется отдельно, то есть имеет свой стек вызовов. Главный поток на дне стека содержит метод `main()`. 
	
2. C помощью унаследования класса Thread. Необходимо переопределить метод run.

### Потоки-демоны и время выполнения
Если у нас есть неважный поток и нам нужно, чтобы он завершился когда основные потоки завершат работу, можно использовать метод setDeamon(). Это делается перед методом start().
```java
timer.setDaemon(true);  
timer.start();
```

==Подсчет времени== можно сделать с помощью System.curretnTimeMillis()


### Чередование потоков
- Чередование потоков можно контролировать с помощью метода `Thread.sleep(long milliSec)`. (поток приостанавливается, и планировщик задач передает управление другому потоку). 
  
  Этот метод может вызвать исключение `InterruptedException`. После приостановки поток станет работоспособным, но не сразу начнет выполнять свою работу.

- Остановить поток нельзя, но можно послать ему сигнал для остановки. Чтобы это сделать, нужно в методе `run()` написать следующее: 
	```java
	run() {
		Thread current = Thread.currentThread(); 
		while (!current.isInterrupted()) { 
		//код для run() 
		} 
	}
	```
	И для остановки работы потока нужно написать `thread.interrupt()`; где `thread` - наш дочерний поток. 


### Мониторы и synchronized()
**Монитор** (объект синхронизации) - хранит состояние "свободно" или "занято" для критической секции.

**Критическая секция** - участок кода, в котором одновременно может находиться только один поток.

Когда мы помечаум у метода класса слово synchronized то в качестве монитора мы используем сам класс:
```java
public synchronized void method() {...}

#that mean:
public void methcod {
	synchronized (this) {...}
}
```

Мы моежм также использовать в качестве монитора любой другой объект:
```java
private Object monitor = new Object();

public void method() {
	synchronized(monitor) {...}
}
```

### [[class ExecutorService]]  
Экзекутер-сервис используется, когда требуется выполнять много разных задач в отдельных потоках. Мы можем поручать экзекутеру-сервису задания, а он будет распределять их по потокам. Если все потоки будут заняты, экзекутер-сервис добавит задания в очередь и отдаст задание первому освободившемуся потоку.

Если нам необходим сделать потоки-демоны для экзекутера, то необходимо в метод для создания ExecutorService передать new ThreadFactory():
```java
ExecutorService service = Executors.newSingleThreadExecutor(new ThreadFactory() {  
    @Override  
 public Thread newThread(Runnable r) {  
        Thread thread = new Thread(r);  
 thread.setDaemon(true);  
 return thread;  
 }  
});
```
### Проблемы потоков
- **Race Condition** (Состояние гонки, Параллелизм) - когда два или более потока имеют доступ к данным одного объекта. Чтобы быть уверенным, что, запустив метод наш поток будет иметь возможность завершить его, прежде чем другой поток сможет начать работу с этими же данными, нужно пометить этот метод словом `synchronized`. Это слово означает, что для работы с отмеченным кодом требуется ключ. Чтобы защитить свои данные, синхронизируйте методы, которые работают с этими данными. 

	*Важно! Если у объекта есть один или несколько синхронизированных методов, поток может выполнять их лишь при наличии ключа к объекту! Такие замки распространяются не на каждый метод отдельно, а на весь объект. *

- **Проблем с кэшированием.** Словом ==volatile== необходимо помечать переменные, с которыми имеют дело два и более потоков. Оно запрещает помещать значение в кэш. Дело в том, что процессор может поместить значение переменной в кэш, и когда поток поменяет значение этой переменной, то для другого потока значение не поменяется (т.к. в медленной памяти значение не поменяется).  
- Race Condition (Состояние гонки) - когда два потока взаимодействуют с одной и той же переменной и меняют ее значение.