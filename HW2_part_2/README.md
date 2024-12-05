Реализация алгоритма template matching.

1) Теоретическая база

Цель алгоритма - найти объект (шаблон) на полной картинке. Для этого шаблон "проходится" по картинке, и на каждой итерация считается некоторая метрика схожести между пикселями на шаблоне и части картинки, на которую идёт наложение. По итогам прохода выбирается часть картинки с наименьшим значением метрики, то есть наиболее похожая. Результатом работы алгоритма является выделенная область, которую можно выделить прямоугольником, наиболее походая на искомый объект.

Также стоит отметить, что поиск осуществляется только между одноканальными картинками, поэтому перед выполнением алгоритма следует перевести изображения в одноканальные.

Существует несколько метрик схожести шаблона и картинки. Наиболее используемые - это сумма квадратов разниц пикселей (SSD) и кросс-корреляция (CCORR). Для изображения f размера ixj и шаблона g размера axb формулы для расчёта этих метрик выглядят следующим образом:

![image](https://github.com/user-attachments/assets/62ceb24e-cade-4042-a8d8-23812fdc8300)

Кроме того, существуют метрики, основанные на нормированных значениях вышеописанных метрик.

2) Описание разработанной системы

В рамках программы была реализована функция template_search, принимающая на вход изображение, шаблон и метрику схожести. Сначала обе картинки переводятся в одноканальное изображение, после чего применяется функция matchTemplate из библиотеки opencv, выдающая координаты области изображения с наименьшим значением метрики схожести с шаблоном. После этого находятся координаты углов прямоугольника, выделяющего эту область, он рисуется на изображении и подаётся на выход.

В дальнейшем каждая из картинок и шаблонов подавалась на вход этой функции и выводилась картинка с её выходом, после чего выяснялось, успешно отработал алгоритм или нет.

3) Результаты работы и тестирования системы.

В качестве метрики расстояния между шаблоном и картинкой было решено выбрать ту, которая корректно отработает на самом простом примере: подаваемый шаблон вырезан из картинки, а сама картинка никак не была изменена. Шаблон и картинка выглядят следующим образом:

![image](https://github.com/user-attachments/assets/ab93e16f-61c9-428a-9089-2c55676fd639)

![image](https://github.com/user-attachments/assets/695eec06-e1f6-4072-9d1d-cba8cdf7ed10)



Из метрик, описанных в п.1, корректно отработал лишь вариант с отнормированной кросс-корреляцией (CV.TM_CCORR_NORMED):

![image](https://github.com/user-attachments/assets/74bbac0d-9705-4f2a-b395-793250539fe4)

В дальнейших экспериментах использовалась именно эта метрика.

В примерах с таким же шаблоном и изменённым (зашумлённым, перевёрнутым и отзеркаленным) изображением найти область, которой соответствовал фрагмент, получилось не во всех случаях:

![image](https://github.com/user-attachments/assets/9f2a6775-ddbf-4047-9a30-ec94940c99cf)

![image](https://github.com/user-attachments/assets/f51ada5a-e66b-4fcb-96bb-25ed46b36180)

![image](https://github.com/user-attachments/assets/e28c7f08-36a7-4163-867e-d9e3e6bdb00c)

Дальнейшие примеры связаны с поиском шаблона, вырезанного с фотографии объекта, на изображении этого объекта, сфотографированного с другого ракурса.

Результаты получились следующие:
1) Шаблон:
   
![image](https://github.com/user-attachments/assets/7c96769f-8bf4-4162-a897-a0ae3e5f99dc)

   Результат:
   
![image](https://github.com/user-attachments/assets/a9f633c5-cfc4-43b4-9981-7daf5f64750e)

2) Шаблон:

![image](https://github.com/user-attachments/assets/9176ea92-c377-4272-9859-37470832df31)

Результат:

![image](https://github.com/user-attachments/assets/036ab769-b686-4c6c-9660-2fc212cc5328)

3) Шаблон:

![image](https://github.com/user-attachments/assets/72ad3b97-9b91-4ecd-b39f-4b353e4f3a31)

  Результат:

![image](https://github.com/user-attachments/assets/61c22d8f-35e1-402d-935b-39790103f07c)

4) Шаблон:

![image](https://github.com/user-attachments/assets/8115a9df-bb13-47c1-812c-d78ea34f1760)

Результат:

![image](https://github.com/user-attachments/assets/e68799aa-6cf1-4368-a8dc-c2382f56c105)

5) Шаблон:

![image](https://github.com/user-attachments/assets/ec68df5c-62f2-4360-baf8-f04d498f862d)

Результат:

![image](https://github.com/user-attachments/assets/3cd72dbc-7032-47b1-bc3f-c927dcdec711)

6) Шаблон:

![image](https://github.com/user-attachments/assets/edaa44d6-2bb1-4947-af8b-d51372dee70d)

Результат:

![image](https://github.com/user-attachments/assets/a8e29c56-93e6-405f-b40e-8c6c3c2feece)

Как можно заметить, алгоритм успешно идентифицировал шаблоны на фотографиях с других ракурсов в 5 из 6 случаев.

4. Выводы по работе:

Таким образом, алгоритм успешно отработал в 8 тестовых примерах из 10. Он хорошо справился с детекцией шаблонов на фотографиях объекта с других ракурсов, на зашумлённых и отзеркаленных фотографиях. Единственный случай, с которым он не справился - это отзеркаленное изображение. Наиболее подходящей метрикой для поиска похожей на шаблон области оказалась нормированная кросс-корреляция.