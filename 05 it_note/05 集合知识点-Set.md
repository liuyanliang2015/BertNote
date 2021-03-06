## set集合
### 概述
用于存储无序(存入和取出的顺序不一定相同)元素，值不能重复。<br>
HashSet是**线程不安全**的，存取速度快，底层是以hash表实现的。
### 用法

   		Set<String> set = new HashSet<String>();
		set.add("a");
		set.add("b");
		set.add("c");
		set.add("d");
		set.add("a");
		set.add(null);
		Iterator<String> it = set.iterator();
		while(it.hasNext()){
			System.out.println(it.next());
		}
		//null,d,b,c,a
 元素的哈希值是通过元素的**hashcode()**方法来获取的。<br>
 HashSet首先判断两个元素的哈希值，如果哈希值一样，接着会比较**equals()**方法,如果equls结果也为true ，HashSet就视为同一个元素。<br>
 如果equals为false就不是同一个元素。

## TreeSet
###概述
用于存储有序(默认对元素自然排序)元素，值不能重复，值不能为空。<br>
底层是红黑树的数据结构，如果比较两个对象的时候返回0，则元素重复。

###用法

        TreeSet<String> set = new TreeSet<String>();
		set.add("d");
		set.add("b");
		set.add("c");
		set.add("a");
		set.add("a");
		//set.add(null);
		Iterator<String> it = set.iterator();
		while(it.hasNext()){
			System.out.println(it.next());
		}
		//a,b,c,d


#### 元素比较(定义比较规则)
A：元素自身具备比较性<br>
元素自身具备比较性，需要元素实现Comparable接口，重写compareTo方法，也就是让元素自身具备比较性，这种方式叫做元素的自然排序也叫做默认排序。

    public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {

String就是实现了Comparable接口的。<br>

代码举例：

  
	import java.util.TreeSet;

	@SuppressWarnings("rawtypes")
	public class Person implements Comparable {
	private String name;
	private int age;
	private String sex;

	public Person(String name, int age, String sex) {
		super();
		this.name = name;
		this.age = age;
		this.sex = sex;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getSex() {
		return sex;
	}

	public void setSex(String sex) {
		this.sex = sex;
	}

	@Override
	public int compareTo(Object obj) {
		Person p = (Person) obj;
		if (this.age > p.age) {
			return 1;
		}

		if (this.age < p.age) {
			return -1;
		}
		return this.name.compareTo(p.name);
	}

	@SuppressWarnings("unchecked")
	public static void main(String[] args) {
		TreeSet ts = new TreeSet();
		ts.add(new Person("aa", 20, "男"));
		ts.add(new Person("bb", 18, "女"));
		ts.add(new Person("cc", 17, "男"));
		ts.add(new Person("dd", 17, "女"));
		ts.add(new Person("dd", 15, "女"));
		ts.add(new Person("dd", 15, "女"));
		System.out.println(ts.size()); // 5
	}
}


B：容器具备比较性
当元素自身不具备比较性，或者自身具备的比较性不是所需要的。那么此时可以让容器自身具备。需要定义一个类实现接口Comparator，重写compare方法，并将该接口的子类实例对象作为参数传递给TreeMap集合的构造方法。<br>
注意：当Comparable比较方式和Comparator比较方式同时存在时，以Comparator的比较方式为主；

    public class Book {
	private String name;
	private String isbn;
	public Book(String name, String isbn) {
		super();
		this.name = name;
		this.isbn = isbn;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getIsbn() {
		return isbn;
	}
	public void setIsbn(String isbn) {
		this.isbn = isbn;
	}
	}



	import java.util.Comparator;
	@SuppressWarnings("rawtypes")
	public class MyComparator implements Comparator {

	@Override
	public int compare(Object o1, Object o2) {
		Book b1 = (Book) o1;
		Book b2 = (Book) o2;
		if(!b1.getIsbn().equals(b2.getIsbn())){
			return 1;
		}
		return b1.getName().compareTo(b2.getName());
	}

	}



	import java.util.TreeSet;
	public class BookTest {
	@SuppressWarnings({ "unchecked", "rawtypes" })
	public static void main(String[] args) {
		TreeSet ts = new TreeSet(new MyComparator());
		ts.add(new Book("think in java", "123123123"));
		ts.add(new Book("java 核心技术", "34236456456"));
		ts.add(new Book("现代操作系统", "45645234234"));
		ts.add(new Book("java就业教程", "234234234"));
		ts.add(new Book("java就业教程", "234234234"));
		System.out.println(ts.size()); //4
	}

	}

### LinkedHashSet

#### 简介
用于存储有序(保证插入顺序)元素，值不能重复。<br>

       Set<String> set = new LinkedHashSet<String>();
		set.add("d");
		set.add("b");
		set.add("c");
		set.add("a");
		set.add("a");
		set.add(null);
		Iterator<String> it = set.iterator();
		while(it.hasNext()){
			System.out.println(it.next());
			// d,b,c,a,null
		}


###总结
看到array，就要想到角标。

看到link，就要想到first，last。

看到hash，就要想到hashCode,equals.

看到tree，就要想到两个接口。Comparable，Comparator。