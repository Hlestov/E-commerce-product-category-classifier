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
##### Обработка изображений
Выбор feature extractor:
Для обработки изображений используется предобученная сеть MaxViT nano 256 без линейного слоя. Данная сеть была выбрана как компромис между качеством и количеством весов модели. Модель выдает неплохие результаты на ImageNet(83% top 1 accuracy), а также имеет всего 16 миллионов параметров, что не может не повлиять на скорость обучения.




 
