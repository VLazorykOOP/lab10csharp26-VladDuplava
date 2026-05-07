using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

namespace CarLife
{
    // ───────────────────────────────────────────────
    // Перелік подій у житті автомобіля
    // ───────────────────────────────────────────────
    public enum CarEventType
    {
        Purchase,       // Купівля
        Refuel,         // Заправка
        OilChange,      // Заміна масла
        TireChange,     // Заміна шин
        Repair,         // Ремонт
        TechnicalCheck, // Техогляд
        Accident,       // Аварія
        Sale            // Продаж
    }

    // ───────────────────────────────────────────────
    // Пріоритет події
    // ───────────────────────────────────────────────
    public enum EventPriority
    {
        Low = 1,
        Medium = 2,
        High = 3,
        Critical = 4
    }

    // ───────────────────────────────────────────────
    // Модель події автомобіля
    // ───────────────────────────────────────────────
    public class CarEvent : IComparable<CarEvent>
    {
        public Guid Id { get; } = Guid.NewGuid();
        public CarEventType Type { get; set; }
        public EventPriority Priority { get; set; }
        public DateTime Date { get; set; }
        public string Description { get; set; } = string.Empty;
        public decimal Cost { get; set; }
        public int Mileage { get; set; }

        public int CompareTo(CarEvent? other)
        {
            if (other is null) return 1;
            // Вищий пріоритет — перший у черзі
            int cmp = other.Priority.CompareTo(Priority);
            return cmp != 0 ? cmp : Date.CompareTo(other.Date);
        }

        public override string ToString() =>
            $"[{Priority,-8}] {Date:dd.MM.yyyy} | {Type,-15} | {Mileage,7} км | {Cost,8:F2} грн | {Description}";
    }

    // ───────────────────────────────────────────────
    // Автомобіль
    // ───────────────────────────────────────────────
    public class Car
    {
        public string Brand { get; set; }
        public string Model { get; set; }
        public int Year { get; set; }
        public string VIN { get; set; }

        public Car(string brand, string model, int year, string vin)
        {
            Brand = brand; Model = model; Year = year; VIN = vin;
        }

        public override string ToString() => $"{Brand} {Model} ({Year}), VIN: {VIN}";
    }

    // ───────────────────────────────────────────────
    // Мінімальна купа (Priority Queue)
    // ───────────────────────────────────────────────
    public class PriorityQueue<T> where T : IComparable<T>
    {
        private readonly List<T> _heap = new();

        public int Count => _heap.Count;

        public void Enqueue(T item)
        {
            _heap.Add(item);
            HeapifyUp(_heap.Count - 1);
        }

        public T Dequeue()
        {
            if (_heap.Count == 0) throw new InvalidOperationException("Черга порожня.");
            T top = _heap[0];
            int last = _heap.Count - 1;
            _heap[0] = _heap[last];
            _heap.RemoveAt(last);
            if (_heap.Count > 0) HeapifyDown(0);
            return top;
        }

        public T Peek() => _heap.Count > 0 ? _heap[0] : throw new InvalidOperationException("Черга порожня.");

        private void HeapifyUp(int i)
        {
            while (i > 0)
            {
                int parent = (i - 1) / 2;
                if (_heap[i].CompareTo(_heap[parent]) >= 0) break;
                (_heap[i], _heap[parent]) = (_heap[parent], _heap[i]);
                i = parent;
            }
        }

        private void HeapifyDown(int i)
        {
            while (true)
            {
                int smallest = i, l = 2 * i + 1, r = 2 * i + 2;
                if (l < _heap.Count && _heap[l].CompareTo(_heap[smallest]) < 0) smallest = l;
                if (r < _heap.Count && _heap[r].CompareTo(_heap[smallest]) < 0) smallest = r;
                if (smallest == i) break;
                (_heap[i], _heap[smallest]) = (_heap[smallest], _heap[i]);
                i = smallest;
            }
        }

        public IEnumerable<T> ToSortedList()
        {
            var copy = new PriorityQueue<T>();
            foreach (var item in _heap) copy.Enqueue(item);
            var result = new List<T>();
            while (copy.Count > 0) result.Add(copy.Dequeue());
            return result;
        }
    }

    // ───────────────────────────────────────────────
    // Статистика
    // ───────────────────────────────────────────────
    public class CarStatistics
    {
        public DateTime From { get; }
        public DateTime To { get; }
        public int TotalEvents { get; private set; }
        public decimal TotalCost { get; private set; }
        public int MileageDelta { get; private set; }
        public Dictionary<CarEventType, int> EventCounts { get; } = new();
        public Dictionary<CarEventType, decimal> CostByType { get; } = new();

        public CarStatistics(DateTime from, DateTime to)
        {
            From = from; To = to;
        }

        public void Add(CarEvent e)
        {
            TotalEvents++;
            TotalCost += e.Cost;

            if (!EventCounts.ContainsKey(e.Type)) EventCounts[e.Type] = 0;
            EventCounts[e.Type]++;

            if (!CostByType.ContainsKey(e.Type)) CostByType[e.Type] = 0;
            CostByType[e.Type] += e.Cost;
        }

        public void SetMileageDelta(int delta) => MileageDelta = delta;

        public void Print()
        {
            Console.WriteLine($"\n{'═',1}{'═' + new string('═', 54)}");
            Console.WriteLine($"  📊 СТАТИСТИКА: {From:dd.MM.yyyy} — {To:dd.MM.yyyy}");
            Console.WriteLine($"{'═',1}{'═' + new string('═', 54)}");
            Console.WriteLine($"  Всього подій   : {TotalEvents}");
            Console.WriteLine($"  Загальні витрати: {TotalCost:F2} грн");
            Console.WriteLine($"  Пробіг за період: {MileageDelta} км");
            Console.WriteLine($"\n  {"Тип події",-20} {"К-сть",6}  {"Сума, грн",12}");
            Console.WriteLine($"  {new string('-', 42)}");
            foreach (var (type, count) in EventCounts.OrderByDescending(x => x.Value))
                Console.WriteLine($"  {type,-20} {count,6}  {CostByType[type],12:F2}");
            Console.WriteLine($"{'═',1}{'═' + new string('═', 54)}\n");
        }
    }

    // ───────────────────────────────────────────────
    // Менеджер "Життя автомобіля"
    // ───────────────────────────────────────────────
    public class CarLifeManager
    {
        public Car Car { get; }
        private readonly PriorityQueue<CarEvent> _queue = new();
        private readonly List<CarEvent> _history = new();

        public CarLifeManager(Car car) => Car = car;

        // Асинхронне додавання події (імітація I/O запису)
        public async Task AddEventAsync(CarEvent carEvent)
        {
            await Task.Delay(10); // симуляція асинхронного запису
            _queue.Enqueue(carEvent);
            Console.WriteLine($"  ✅ Подію додано у чергу: {carEvent.Type} [{carEvent.Priority}]");
        }

        // Асинхронна обробка всієї черги
        public async Task ProcessAllAsync()
        {
            Console.WriteLine("\n  🔄 Обробка черги подій за пріоритетом...\n");
            while (_queue.Count > 0)
            {
                var ev = _queue.Dequeue();
                await Task.Delay(20); // симуляція обробки
                _history.Add(ev);
                Console.WriteLine($"  ▶ Оброблено: {ev}");
            }
            Console.WriteLine("\n  ✔ Усі події оброблено.\n");
        }

        // Статистика за певний період
        public CarStatistics GetStatistics(DateTime from, DateTime to)
        {
            var stats = new CarStatistics(from, to);
            var events = _history
                .Where(e => e.Date >= from && e.Date <= to)
                .OrderBy(e => e.Date)
                .ToList();

            foreach (var e in events) stats.Add(e);

            if (events.Count >= 2)
                stats.SetMileageDelta(events.Last().Mileage - events.First().Mileage);

            return stats;
        }

        public void PrintHistory()
        {
            Console.WriteLine($"\n  📋 ІСТОРІЯ ПОДІЙ ({_history.Count}):");
            Console.WriteLine($"  {new string('-', 70)}");
            foreach (var e in _history.OrderBy(x => x.Date))
                Console.WriteLine($"  {e}");
            Console.WriteLine($"  {new string('-', 70)}\n");
        }
    }

    // ───────────────────────────────────────────────
    // Точка входу
    // ───────────────────────────────────────────────
    class Program
    {
        static async Task Main()
        {
            Console.OutputEncoding = System.Text.Encoding.UTF8;
            Console.WriteLine("╔══════════════════════════════════════╗");
            Console.WriteLine("║       ЖИТТЯ АВТОМОБІЛЯ  🚗           ║");
            Console.WriteLine("╚══════════════════════════════════════╝\n");

            // 1. Створення автомобіля
            var car = new Car("Toyota", "Camry", 2018, "JT2BF22K1W0123456");
            Console.WriteLine($"  🚘 Автомобіль: {car}\n");

            var manager = new CarLifeManager(car);

            // 2. Асинхронне додавання подій з різними пріоритетами
            Console.WriteLine("  ➕ Додавання подій у чергу...\n");

            var events = new[]
            {
                new CarEvent { Type = CarEventType.Purchase,       Priority = EventPriority.High,     Date = new DateTime(2018,3,15),  Cost = 620000, Mileage = 0,     Description = "Купівля нового авто" },
                new CarEvent { Type = CarEventType.Refuel,         Priority = EventPriority.Low,      Date = new DateTime(2018,4,1),   Cost = 1200,   Mileage = 800,   Description = "Перша заправка" },
                new CarEvent { Type = CarEventType.OilChange,      Priority = EventPriority.Medium,   Date = new DateTime(2019,3,10),  Cost = 850,    Mileage = 10000, Description = "Планова заміна масла" },
                new CarEvent { Type = CarEventType.Accident,       Priority = EventPriority.Critical, Date = new DateTime(2020,7,22),  Cost = 18000,  Mileage = 35000, Description = "ДТП — удар ззаду" },
                new CarEvent { Type = CarEventType.Repair,         Priority = EventPriority.High,     Date = new DateTime(2020,8,5),   Cost = 15000,  Mileage = 35200, Description = "Ремонт після ДТП" },
                new CarEvent { Type = CarEventType.TireChange,     Priority = EventPriority.Medium,   Date = new DateTime(2021,10,15), Cost = 4800,   Mileage = 52000, Description = "Зимова гума" },
                new CarEvent { Type = CarEventType.TechnicalCheck, Priority = EventPriority.High,     Date = new DateTime(2022,3,20),  Cost = 600,    Mileage = 68000, Description = "Обов'язковий техогляд" },
                new CarEvent { Type = CarEventType.OilChange,      Priority = EventPriority.Medium,   Date = new DateTime(2023,5,11),  Cost = 950,    Mileage = 90000, Description = "Позапланова заміна масла" },
                new CarEvent { Type = CarEventType.Refuel,         Priority = EventPriority.Low,      Date = new DateTime(2024,1,8),   Cost = 1500,   Mileage = 112000,Description = "Заправка на трасі" },
                new CarEvent { Type = CarEventType.Sale,           Priority = EventPriority.High,     Date = new DateTime(2024,6,30),  Cost = -280000,Mileage = 125000,Description = "Продаж автомобіля" },
            };

            // Паралельне асинхронне додавання
            await Task.WhenAll(events.Select(e => manager.AddEventAsync(e)));

            // 3. Обробка черги за пріоритетом
            await manager.ProcessAllAsync();

            // 4. Виведення повної історії
            manager.PrintHistory();

            // 5. Статистика за різні періоди
            var stats2020_2022 = manager.GetStatistics(new DateTime(2020, 1, 1), new DateTime(2022, 12, 31));
            stats2020_2022.Print();

            var statsAll = manager.GetStatistics(new DateTime(2018, 1, 1), new DateTime(2024, 12, 31));
            statsAll.Print();

            Console.WriteLine("  Натисніть будь-яку клавішу для виходу...");
            Console.ReadKey();
        }
    }
}
