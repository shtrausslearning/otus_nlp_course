### Stemming

<code>Стемминг</code> (англ. stemming — находить происхождение) — это процесс нахождения основы слова для заданного исходного слова. Основа слова не обязательно совпадает с морфологическим корнем слова и не обязана являться существующим словом в языке. Стемминг – это грубый эвристический процесс, который отрезает «лишнее» от корня слов, часто это приводит к потере словообразовательных суффиксов


```python

# [1] Stemming (rus)
# слушать слушаю слушаем -> слуша 
 
from nltk.stem.snowball import SnowballStemmer 
stemmer = SnowballStemmer("russian")

text = 'в этот вечер мы слушаем вебинар по обработке естественного языка в отус'
stemmed_text = ' '.join([stemmer.stem(x) for x in text.split(' ')])

```

### Lemmatisation

<code>Лемматизация</code> приводит все встречающиеся словоформы к одной, нормальной словарной форме. **Лемматизация** использует словарь и морфологический анализ, чтобы в итоге привести слово к его канонической форме – **лемме**.

```python

# [2] Stemming (eng)
# в этот вечер мы [слуша] вебинар по обработк естествен язык в отус

from nltk.stem.snowball import SnowballStemmer 
stemmer = SnowballStemmer("english")
text = 'tonight we are listening to a webinar in otus'
stemmed_text = ' '.join([stemmer.stem(x) for x in text.split(' ')])

# 'tonight we are [listen] to a webinar in otus'

```


```python

# [3] Lemmatisation (rus)

import pymorphy2 
morph = pymorphy2.MorphAnalyzer() # Морфологический анализатор

text = 'в этот вечер мы слушаем слушать слушали вебинар по обработке естественного языка в отус'
stemmed_text = ' '.join([morph.parse(x)[0].normal_form for x in text.split(' ')])

# в этот вечер мы [слушать] [слушать] [слушать] вебинар по обработка естественный язык в отус

```
