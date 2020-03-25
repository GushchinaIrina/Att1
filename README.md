# Att1
using System.Collections.Generic;
using System.Collections;

namespace ClassLibrary1
{
    public interface MySet<T> : IEnumerable<T>              //базовый интерфейс для всех списков
    {
        //методы:
        void Add(T Value);                                 //добавление элемента
        void Clear();                                      //удаление всех элементов
        bool Contains(T Value);                            //проверить есть элемент или нет
        void Remove(T Value);                              //удалить элемент
       // MySet<T> Union(MySet<T> aSet);

        //свойства:
        int Count { get; set; }                            //кол-тво элементов в списке
        bool isEmpty { get; set; }                         //проверка на пустоту
        //T[] Elements { get;  }                              //массив эл-тов мн-тва
    }

    public class DoublyNode<T>                                 //класс 2-го узла, который предст-ет элемент списка
    {
        public DoublyNode(T Value)
        {
            Data = Value;
        }
        public T Data { get; set; }                            //данные элемента
        public DoublyNode<T> Prev { get; set; }                //"ссылка" на пред-щий элемент
        public DoublyNode<T> Next { get; set; }                //"ссылка" на след-щий элемент
    }

    public  class LinkedSet<T> : MySet<T>                        //класс множества на основе 2-го списка
    {
        DoublyNode<T> head;                                    // головной/первый элемент
        DoublyNode<T> tail;                                    // последний/хвостовой элемент

        public int Count { set; get; }                         //кол-тво эл-тов в списке
        public bool isEmpty { set; get; }                      //проверка на пустоту
     
        public void Add(T Value)                               //метод добавления элемента 
        {
            DoublyNode<T> node = new DoublyNode<T>(Value);

            if (head == null)                                  //если нет первого узла
            {
                head = node;                                   //головной элемент становится узлом
            }
            else
            {
                tail.Next = node;                              //свойство Next указывает на хвостовой эл-нт, который до этого хранился в узле
                node.Prev = tail;                              //свойство Prev указывает на узел, который до этого хранился в хвостовом эл-те
            }
            tail = node;                                       //хвостовой элемент становится узлом
            Count++;                                           //увел-ем кол-тво элементов
           
        }

        public void Clear()                                    //метод удаления всех элементов
        {
            head = null;
            tail = null;
            Count = 0;
        }

        public bool Contains(T Value)                         //метод проверки сущ-ния элемента
        {
            DoublyNode<T> current = head;                     //тукущий элемент явл-ся головным
            while (current != null)
            {
                if (current.Data.Equals(Value))               //если заданный объект == текущему объекту
                    isEmpty = true;                                   // isEmpty = true;
                current = current.Next;                       //текущий элемент стан-ся послед-щим
            }
             if(isEmpty != true)
                isEmpty = false;

            return isEmpty;
        }
        
        public void Remove(T Value)                           //метод удаления заданного элемента
        {
            DoublyNode<T> current = head;                     //текущий элемент явл-ся головным

            while (current != null)
            {
                if (current.Data.Equals(Value))               //если заданный объект == текущему объекту
                {
                    break;
                }
                current = current.Next;                       //текущий элемент стан-ся послед-щим
            }
            if (current != null)
            {

                if (current.Next != null)                     // если узел не последний
                {
                    current.Next.Prev = current.Prev;         //то послед-щий от пред-щего стан-тся пред-щим
                }
                else
                {
                    tail = current.Prev;                      // если последний, переустанавливаем пред-щий эл-нт в tail
                }

                if (current.Prev != null)                      //если узел не первый
                {
                    current.Prev.Next = current.Next;         //то пред-щий от посл-щего стан-тся послед-щим
                }
                else
                {
                    head = current.Next;                      //иначе головным эл-том становится послед-щий
                }
                Count--;                                     //уменьшаем кол-тво эл-тов

            }

        }

       IEnumerator IEnumerable.GetEnumerator()
        {
             return ((IEnumerable)this).GetEnumerator();
        }

        IEnumerator<T> IEnumerable<T>.GetEnumerator()
         {
             DoublyNode<T> current = head;
             while (current != null)
             {
                yield return current.Data;
                current = current.Next;
             }
        }   
    }
 }

using ClassLibrary1;
using System;

namespace ConsoleApp2
{
    class Program
    {
        static void Main(string[] args)
        {
            int num, x;
            bool w = true;
            
            MySet<Object> set = new LinkedSet<Object>();
           
            Console.Write("Выберите действие: \n\t1-добавить элемент; \n\t2-удалить все элементы; " +
                "\n\t3-проверить элемент на сущ-ние; \n\t4-удалить элемент; \n\t5-показать содержимое списка; \n\t6-завершить работу.\n");
            
            while (w)
            {
                num = Convert.ToInt32(Console.ReadLine());

                switch (num)
                {
                    case 1:
                        Console.WriteLine("Введите добавляемое значение:");
                        x = Convert.ToInt32(Console.ReadLine());
                        set.Add(x);
                        Console.WriteLine("Значение " + x + " добавлено в список;\n");
                        break;

                    case 2:
                        set.Clear();
                        Console.WriteLine("Список пуст!\n");
                        break;

                    case 3:
                        Console.WriteLine("Введите значение, которое хотите проверить на сущ-ние:");
                        x = Convert.ToInt32(Console.ReadLine());
                        if (set.Contains(x) == true)
                            Console.WriteLine("Данное значение содержится в списке;\n");
                        else
                            Console.WriteLine("Данное значение НЕ содержится в списке;\n");
                        break;

                    case 4:
                        Console.WriteLine("Введите значение, которое хотите удалить:");
                        x = Convert.ToInt32(Console.ReadLine());
                        if (set.Contains(x) == true)
                        {
                            set.Remove(x);
                            Console.WriteLine("Значение " + x + " удалено из списка;\n");
                        }
                        else
                            Console.WriteLine("Значение " + x + " невозможно удалить, т.к. его нет в списке;\n");
                        break;

                    case 5:
                       Console.WriteLine("Содержимое списка:");
                        
                        foreach (var item in set)
                        {
                            Console.WriteLine(item);
                        }
                       
                        Console.WriteLine("\n");
                        break;

                    case 6:
                        w = false;
                        break;
                }
            }
        }
    }
 }

using Microsoft.VisualStudio.TestTools.UnitTesting;
using ClassLibrary1;

namespace UnitTestProject1
{
    [TestClass]
    public class UnitTest1
    {
        [TestMethod]
        public void NumberOfItemsAfterAdding()
        {
            //arange
            int x = 10;
            int y = 15;
            int z = 20;
            
            //act
            LinkedSet<int> set = new LinkedSet<int>();
            set.Add(x);
            set.Add(y);
            set.Add(z);

            int expected = 3;

            //assert
            Assert.AreEqual(expected, set.Count);
        }

        [TestMethod]
        public void NumberOfItemsAfterDeletion()
        {
            //arange
            int x = 10;
            int y = 15;
            int z = 20;

            //act
            LinkedSet<int> set = new LinkedSet<int>();
            set.Add(x);
            set.Add(y);
            set.Add(z);
            set.Remove(y);

            int expected = 2;

            //assert
            Assert.AreEqual(expected, set.Count);
        }

        [TestMethod]
        public void CheckingForTheContentsOfValue1()
        {
            //arange
            int x = 10;
            
            //act
            LinkedSet<int> set = new LinkedSet<int>();
            set.Add(x);
            bool expected = true;

            //assert
            Assert.AreEqual(expected, set.Contains(x));
        }

        [TestMethod]
        public void CheckingForTheContentsOfValue2()
        {
            //arange
            int x = 10;
            int y = 15;

            //act
            LinkedSet<int> set = new LinkedSet<int>();
            set.Add(x);
            bool expected = false;

            //assert
            Assert.AreEqual(expected, set.Contains(y));
        }

        [TestMethod]
        public void NumberOfItemsAfterDeletingAll()
        {
            //arange
            int x = 10;
            int y = 15;

            //act
            LinkedSet<int> set = new LinkedSet<int>();
            set.Add(x);
            set.Add(y);
            set.Clear();

            int expected = 0;

            //assert
            Assert.AreEqual(expected, set.Count);
        }
    }
}
