---
title: پیشبینی نواسانات قیمت طلا با Python
author: Hassan Abedi
excerpt: پیشبنی معیارهای لحظه‌ای طلا در بازار طلای ایران
---

## مقدمه
{: .rtl}

طلا یک فلز گرانبها در تمام دنیاست، طلا را میتوان به صورت لحظه‌ای خرید و فروش کرد. بخاطر بالا بودن ارزش طلا تغییر قیمت در حد حتی چند تومان هم در بازار طلا میتواند باعث سود و یا ضرر چند میلیونی برای فرد فروشنده و یا خریدار گردد. در صورتی که بتوان نواسان قیمت این فلز گرانبها را با دقت قابل قبولی پیش از وقوع این نوسانات یشبینی نمود، می توان امیدوار بود که به سود زیادی دست پیدا کرد. در این نوشته به بررسی این مساله می پردازیم که چگونه میتوان به کمک روشهای متداول تحلیل داده و بکارگیری زبان [پایتون](https://www.python.org) مدل ساده‌ای برای پیشبنی نواسانات قیمت طلا در بازار ایران ساخت.
{: .rtl}

# داده‌ی استفاده شده
{: .rtl}

در این کار از داده‌ی لحظه‌ای قیمت ارزهای درهم امارات و دلار آمریکا به همراه اطلاعات مثقال طلا در همان لحظه استفاده شده است.
اطلاعات جمع آوری شده مربوط به بازه‌ی چهار ماهه‌ی ۷ آوریل ۲۰۱۷ تا ۱۰ ژوییه ی ۲۰۱۷ می باشند. اطلاعات روزانه و از ساعت ۱۱ تا ۲۰ هر روز در دسترس بوده اند و جمع آوری شده اند. با فاصله هر ۱ دقیقه ۵ معیار Open، Close، Low، High و Volume برای درهم، دلار و مثقال طلا ثبت و ذخیره شده است. در بعضی از لحظه‌ها اطلاعات ۵ معیار ذکر شده برای درهم و دلار در دسترس نمی بوده است، در این صورت مقدار ۵ معیار ذکر شده برای این ۵ معیار برابر مقدار Not Available در نظر گرفته شده است. مجموعه داده‌ی مورد بحث [از اینجا](http://habedi.me/datasets/gold/gold_data_20170407to20170810.csv) قابل دانلود است. ضمنا مبنای ۴ معیار Open، Close، Low و High ریال ایران می باشد.
{: .rtl}



```python
import pandas

# don't display more than 20 rows in the output
pandas.set_option('display.max_rows', 20)
```


```python
# change dataset path to where the data is stored on your machine
gold_dataset_path = "./gold_data_20170407to20170810.csv"
gold_data = pandas.read_csv(gold_dataset_path, parse_dates=['date'])
```


```python
# let's have a look at top 10 first rows
gold_data.iloc[0:10,]
```




<div class="table-wrapper">
    
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>time</th>
      <th>derham.close</th>
      <th>derham.high</th>
      <th>derham.low</th>
      <th>derham.open</th>
      <th>derham.volume</th>
      <th>dollar.close</th>
      <th>dollar.high</th>
      <th>dollar.low</th>
      <th>dollar.open</th>
      <th>dollar.volume</th>
      <th>iran_gold.close</th>
      <th>iran_gold.high</th>
      <th>iran_gold.low</th>
      <th>iran_gold.open</th>
      <th>iran_gold.volume</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>20170228</td>
      <td>151200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20170228</td>
      <td>151300</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20170228</td>
      <td>151400</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20170228</td>
      <td>151500</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>20170228</td>
      <td>151600</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>20170228</td>
      <td>151700</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20170228</td>
      <td>151800</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>20170228</td>
      <td>152000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>20170228</td>
      <td>152100</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>20170228</td>
      <td>152200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>5035000</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



```python
from matplotlib import pyplot as plt
```


```python
x = gold_data["date"]
y_gold = gold_data["iran_gold.high"]
y_derham = gold_data["derham.high"]
y_dollar = gold_data["dollar.high"]

plt.figure(figsize=(20, 10))
plt.plot(x, y_derham, color="green")
plt.gca().set(xlabel='date', ylabel='derham.high', title="UAE Derham Price Over Time")
plt.show()

plt.figure(figsize=(20, 10))
plt.plot(x, y_dollar, color="blue")
plt.gca().set(xlabel='date', ylabel='dollar.high', title="US Dollar Price Over Time")
plt.show()

plt.figure(figsize=(20, 10))
plt.plot(x, y_gold, color="red")
plt.gca().set(xlabel='date', ylabel='iran_gold.high', title="Gold Price Over Time")
plt.show()
```


![UAE Derham Price Over Time](http://habedi.me/datasets/gold/figures/DerharmOverTime.png)



![US Dollar Price Over Time](http://habedi.me/datasets/gold/figures/DollarOverTime.png)



![Gold Price Over Time](http://habedi.me/datasets/gold/figures/IRGoldOverTime.png)


