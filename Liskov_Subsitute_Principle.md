LSP'ye göre herhangi bir sub-class base classın yerine kullanılabilmelidir. Bu prensibe göre base class ve sub class arasında herhangi bir fark yoktur. Base class kullanılarak oluşturulan metotlar için sub claass olan nesnelerde aynı davranışı sergilemelidir, çünkü oluşturulan metotlar base class davranışları örnek alınarak oluşturulmuştur. Sub classlarda meydana gelen davranış değişiklikleri, bu metotların hatalı çalışmasına sebp olabilir. Özellikle bu metotlarda instance of gibi nesnelerin tipleri arasında kıyaslama yapılmak zorunda kalındığı zaman, LSP prensibi yok sayılmış olur ve bu durum sub classların varlığından varlığından haberdar olunduğu anlamına gelir. Base classlar ideal durumda sub classların varlığından haberdar olmamalıdır. 

LSP prensibi Open Closed prensibinin özel bir türüdür desek yanlış olmaz. OCP’de de olduğu gibi LSPde de genişlemeye açık yapılar söz konusudur. LSP ilk bakışta anlaşılması zor olsada, altında yatan ana fikri: sub classlardan oluşan nesnelerin üst sınıfın nesneleri ile yer değiştirdikleri zaman, aynı davranışı sergilemesini beklemektir.

Örnek bir kod ile açıklayacak olursak;

Öncelikle base class olarak bir "Duck" abstract classımız olsun ve bu classın Quack (vaklama), Swim (Yüzme), Fly (Uçma) metodları olsun, aynı zamanda bu base classı extend eden MallarDuck (Yeşil Ördek), MarbleDuck (Yaz Ördeği) ve RubberDuck (Plastik Ördek) olsun

                public abstract class Duck{
                    
                    public abstract void Quack();
                    
                    public abstract void Swim();
                    
                    public abstract void Fly();
                }

                public class MallardDuck : Duck{
                    public override void Quack()
                    {
                        System.Console.WriteLine("Quack!");
                    }

                    public override void Swim()
                    {
                        System.Console.WriteLine("Swimming!");
                    }

                    public override void Fly()
                    {
                        System.Console.WriteLine("Flying!");
                    }
                }

                public class MarbledDuck : Duck{
                    public override void Quack()
                    {
                        System.Console.WriteLine("Quack!");
                    }

                    public override void Swim()
                    {
                        System.Console.WriteLine("Swimming!");
                    }

                    public override void Fly()
                    {
                        System.Console.WriteLine("Flying!");
                    }
                }

                public class RubberDuck : Duck{
                    public override void Quack()
                    {
                        System.Console.WriteLine("Squeak!");
                    }

                    public override void Swim()
                    {
                        System.Console.WriteLine("Floating!");
                    }

                    public override void Fly()
                    {
                        throw new Exception("Rubber ducks can't fly!");
                    }
                }

    Örnek kodlarımıza baktığımızda RubberDuck'ın uçamadığını ve bu sebeple bir exception fırlattığını görürüz. LSP'yi tekrar hatırlayacak olursak bir sub class, base classın yerine geçtiğinde aynı davranışları sergilemelidir demiştik. Ancak RubberDuck uçamadığı için bu prensibin dışına çıkmış oluyoruz. 

    LSP prensibini uygulayarak ilgili classları oluşturacak olursak, öncelikle Quack, Swim, Fly metotlarımızı birer interface olarak tanımlayalım. 

                public interface IFly
                {
                    void Fly();
                }

                public interface ISwim
                {
                    void Swim();
                }

                public interface IQuack
                {
                    void Quack();
                }

            public class MallardDuck : IFly, ISwim, IQuack
            {
                public void Quack()
                {
                    System.Console.WriteLine("Quack!");
                }

                public void Swim()
                {
                    System.Console.WriteLine("Swimming!");
                }

                public void Fly()
                {
                    System.Console.WriteLine("Flying!");
                }
            }

            public class MarbledDuck : IFly, ISwim, IQuack
            {
                public void Quack()
                {
                    System.Console.WriteLine("Quack!");
                }

                public void Swim()
                {
                    System.Console.WriteLine("Swimming!");
                }

                public void Fly()
                {
                    System.Console.WriteLine("Flying!");
                }
            }

            public class RubberDuck : ISwim, IQuack
            {
                public void Quack()
                {
                    System.Console.WriteLine("Squeak!");
                }

                public void Swim()
                {
                    System.Console.WriteLine("Floating!");
                }
            }

            Yukarıdaki kodlarda görüleceği üzere Duck classını extend eden sub classlara Interfaceler yardımı ile yapabilecekleri özellikleri tanımladık ve bu sayede RubberDuck'a uçna özelliği vermemize gerek kalmadı, LSP prensibinde belirtildiği gibi her sub class base class ile aynı davranışları sergileyecek şekilde oluşturuldu ve ekstra davrnışlar interfaceler ile tanımlanmış oldu.