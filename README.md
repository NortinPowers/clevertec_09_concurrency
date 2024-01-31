<h1 align="center">Clevertec course<h1>
<h2 align="center">Concurrency</h2>
<h3>Описание задачи</h3>

1. Создать любой gradle проект <br>
2. Придерживаться GitFlow: master -> develop -> feature/fix <br>
3. Создать два класса:
- Клиент - имеет список данных в виде List &lt;Integer&gt; от 1 до n.  Отдельными потоками, по случайному индексу из списка выбирается значение (метод remove()) и в виде запроса (класс с int -полем), содержащего это значение, отправляется на сервер в асинхронном режиме (например отправляются со случайной задержкой между запросами - диапазон - от 100 до 500 мс). Количество запросов равно размеру первоначального списка. Контроль: после отправки всех запросов размер списка данных = 0
- Сервер - получает запросы от клиента. Метод обрабатывающий запрос имеет задержку в виде рандомного инта. Диапазон - от 100 до 1000 мс. Сервер обрабатывает запросы, используя общий для всех потоков ресурс: List<Integer>, в который складываются значения приходящие с запросом. В ответ от сервера передаем размер листа на момент формирования ответа (класс с int-полем). Итоговый контроль правильности данных на стороне сервера: список (общий ресурс) должен содержать значения от 1 до n без пробелов, повторений, размерность его должна составлять n
4. Клиент получает от сервера ответ и в общий для всех потоков ресурс accumulator суммирует значение из ответа от сервера. Итоговый контроль: accumulator = (1+n) * (n/2). Т.е. для диапазона 1-100 ответ должен быть 5050 <br>
5. Протестировать эти два класса с проверкой многопоточности <br>
6. Протестировать взаимодействие клиента - сервера отдельным тестом (интеграционный) - обязательно <br>
7. В реализации использовать классы пакета java.util.concurrent (обязательно Lock, Callable, Executor, Future, остальное - по выбору) <br>
8. Методы класса Object (относящиеся к потокам и монитору) и ключевое слово synchronized НЕ использовать <br>

<h3>Использование</h3>
1. Создание класса Server:
- Server server = new Server(); // Сервер не требует параметров
2. Создание класса Client:
- int numRequests = 25; // Выбранное Вами число запросов в диапазоне 1-100
- Client client = new Client(server, numRequests);
<h3>Контрольные точки</h3>
1. После выполнения всех запросов от клиента, размер списка данных на сервере должен быть равен 0.
2. Список данных на сервере должен содержать уникальные значения от 1 до n.
3. Размер списка данных на сервере должен быть равен n.
- int accumulator = client.getAccumulator();
- int expectedAccumulator = ((1 + numRequests) * (numRequests )/ 2);
- if (accumulator == expectedAccumulator) {
- System.out.println("Тест пройден!");
- showInfo(accumulator, expectedAccumulator, client); \\\\*
- } else {
- showInfo(accumulator, expectedAccumulator, client);
- }
- \* 
- private static void showInfo(int accumulator, int expectedAccumulator, Client client) {
- System.out.println("accumulator = " + accumulator);
- System.out.println("expectedAccumulator = " + expectedAccumulator);
- System.out.println("Размер списка данных = " + client.getData().size());
- }
<h3>Тесты</h3>
Для запуска тестов используйте gradle test <br>
Учтите, что параметризованный тест из ClientServerTest требует достаточное количество времени для завершения
