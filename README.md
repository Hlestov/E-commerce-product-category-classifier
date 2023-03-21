# E-commerce-product-category-classifier

Стэк: 
 -  pandas🐼
 -  matplotlib📊
 -  sklearn🍊+🔵
 -  pytorch🔦
 -  timm🌺
 -  transformers🤖

## Этапы построения модели классификации продуктов
1. Разведочный анализ данных
2. Создание векторного описания карточки товара:
   - обработка изображений
   - обработка текстовых данных
3. Обучение модели и подбор гиперпараметров

### EDA 
Разведочный анализ показал, что в данных нет пропусков и выбросов, однако присутствует существенный дисбаланс классов. Есть классы у которых более 6000 приверов, есть классы у которых только один пример. Это может негативно повлиять на обобщающую способность алгоритма.  
![image](https://user-images.githubusercontent.com/52448692/226642989-bcf0e28f-acba-4103-9100-04dadb9d07d3.png)

### Создание векторного описания карточки товара

#### Обработка изображений
Выбор feature extractor:  
Для обработки изображений используется предобученная сеть MaxViT nano 256 без линейного слоя. Данная сеть была выбрана как компромис между качеством и количеством весов модели. Модель выдает неплохие результаты на ImageNet(83% top 1 accuracy), а также имеет всего лишь 16 миллионов параметров, что не может не повлиять на скорость обучения. В дальнейшем для улучшения классификации можно использовать более "тяжелые" сети для увелечения метрик качества, либо более "легкие" у которых будет выше скорость обработки данных. Выбор модели зависит от количества данных, которых ей нужно будет обрабатывать, а также доступных вычислительных мощностей.

Использование модели:  
Изображения были пропущенны через модель, далее я "обучил" KNN только на признаках изображений. KNN - один из самых простых методов для проверки качества признаков, так как он не требует времени для обучения и показывает насколько хорошо наши данные расположенны в новом пространстве. KNN дал F1=0.51. Неплохой показатель, это означает, что модель справилась с задачей извлечения новых признаков. 

Как улучшить признаки?  
Показатель F1 на признаках изображений можно улучшить, для этого необходимо обратно "прикрутить" линейный слой и обучить классификатор на этих изображениях. Модель научится извлекать более качественные признаки для нашей задачи. Главное следить за train и valid выборками, чтобы не произошло утечки данных.

#### Обработка текстовых данных
Я разделил обработку текстовых данных на две части:
- обработка title
- обработка description


Выбор feature extractor
Выбор модели для обработки текстовых данных осуществлялся на основе статьи https://habr.com/ru/post/669674/. Sbert_large обладает большей скоростью, по сравнению с другими рускоязычными энкодерами, поэтому я выбрал его как для title так и для description. При необходимости можно выбрать более "тяжелую" модель для лучшего качества.

Title:  
Title - это название товара, поэтому логично, что именно признаки из названия могут внести существенный вклад в метрики. После обработки title моделью и обучения KNN F1=0.75 на валидационной выборке.

Description:  
Все действия аналогичны. F1=0.61 при использовании KNN. 

Как улучшить признаки?  
Точно также как и модель для обработки изображений. "Прикручиваем" линейный слой и обучаем модель. Опять же главное заранее разделить данные на train и valid, чтобы не было утечки данных.

### Обучение модели и подбор гиперпараметров

Я объеденил все полученные признаки по изображениям, description и title. KNN дал F1=0.789. MLP дал F1=0.82. Также для улучшения метрики я пытался использовать sampling во всевозможных вариациях Random under и under sampling в лучшем случае ничего не улучшали, но чаще всего только портили. 

## Резюме












 
