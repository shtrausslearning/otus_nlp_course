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

### Lemmatise JSON text data

Load JSON data

```python

with open('/kaggle/input/khazah-news/train.json', encoding = 'utf-8') as json_file:
    data = json.load(json_file)

```

Preprocess text data

```python

# функция которая находит только слова
def words_only(text):
    try:
        return " ".join(re.findall(r'[А-Яа-яA-zёЁ-]+',text)).lower()
    except:
        return ""

# расширим список стоп-слов, словами, которые являеются стоп-словами в данной задаче
add_stop_words = ['kz', 'казахстан', 'астана', 'казахский', 'алматы', 'ао', 'оао', 'ооо']
months = ['январь', 'февраль', 'март', 'апрель', 'май', 'июнь', 'июль', 'август', 'сентябрь', 'октябрь', 'ноябрь', 'декабрь',]
all_stop_words = stop_words + add_stop_words + months

def process_data(data):
    
    texts = []
    targets = []
    
    # поочередно проходим по всем новостям в списке
    for item in tqdm(data):
               
        text_lower = words_only(item['text'])            # оставим только слова (str)
        tokens     = word_tokenizer.tokenize(text_lower) # разбиваем текст на слова (lst of str)
        
        # удаляем пунктуацию и стоп-слова
        tokens = [word for word in tokens if (word not in all_stop_words and not word.isnumeric())]
        
        texts.append(tokens) # добавляем в предобработанный список
    
    return texts


y = [item['sentiment'] for item in data]
texts = process_data(data)

```

Lemmatise all text

```python

from tqdm import tqdm_notebook

# применяем лемматизацию ко всем текстам
for i in tqdm_notebook(range(len(texts))):          
    text_lemmatized = [morph.parse(x)[0].normal_form for x in texts[i]] # применяем лемматизацию для каждого слова в тексте
    texts[i] = ' '.join(text_lemmatized)                # объединяем все слова в одну строку через пробел
    
```
