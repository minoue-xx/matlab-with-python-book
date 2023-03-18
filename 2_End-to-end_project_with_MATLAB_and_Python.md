
# 2. MATLAB と Python を使った End-to-end プロジェクト: End-to-end project with MATLAB & Python

MathWorks に入社して Heather と出会いました。彼女は MATLAB と Python を一緒に使う方法を紹介するデモを開発していました。ここでは、彼女が開発した**天気予報のアプリ**を紹介します。コードは彼女の GitHub のレポ（[https://github.com/hgorr/weather-matlab-python ](https://github.com/hgorr/weather-matlab-python )）で公開されています。

ZIPをダウンロードするか、リポジトリをクローンして、コードを取得することから始めます。

```matlab
!git clone https://github.com/hgorr/weather-matlab-python
cd weather-matlab-python\
```

出来上がったアプリケーションはこのような感じになります。

<img src="./media/image8.png" alt="Chart, line chart" />

次のようなステップで作業していきます。

   1.  Heather の python コードを呼び出して気象データを取得する。 
   1.  大気環境を予測する MATLAB モデルを統合する。
   1.  MATLAB + Python で作られたアプリケーションをデプロイする。

この例では、[openweathermap.org](https://openweathermap.org/) のWebサービスからのデータを使用します。


<img src="./media/image9.png" alt="Graphical user interface" />

このライブデータにアクセスするためには、無料版のサービスに[登録](https://home.openweathermap.org/users/sign_up)する必要があります。その後、API key を生成するためのオプションが表示されます: <https://home.openweathermap.org/api_keys>

<img src="./media/image10.png" alt="website" />

この key は、ウェブサービスを呼び出すたびに必要となります。例えば、現在の天気（[current weather](https://openweathermap.org/current)）をリクエストするには、次のアドレスを呼び出すことで実行されます。

`api.openweathermap.org/data/2.5/weather?q={city name}\&appid=`[`{API key}`](https://home.openweathermap.org/api_keys)

[API key](https://home.openweathermap.org/api_keys) を <u>accessKey.txt</u> という名前のテキストファイルに保存します。

```matlab
% apikey = fileread("accessKey.txt");
```

またはこのスクリプトのようにサンプル API key を使用することもできます。

```matlab
appid ='b1b15e88fa797225412429c1c50c122a1';
```

# 2.1. MATLAB から Python を呼び出す: Call Python from MATLAB

Heather は [weather.py](https://github.com/hgorr/weather-matlab-python/blob/main/weather.py) というモジュールを作成し、Web サービスから読み込み、それが返す JSON データをパースしています。もちろん MATLAB でもできますが、Python からデータにアクセスする例として、このモジュールを使ってみましょう。

## 2.1.1. Python のインストールを確認する: Check the Python installation

まず [pyenv](https://www.mathworks.com/help/matlab/ref/pyenv.html) コマンドで Python 環境に接続します。MATLAB と Python のセットアップ方法については、次章をご覧ください。MATLABは、ベースとなる Python、インストールしたパッケージ、独自の Python コードから、Python 関数を呼び出し、Python オブジェクトを作成することができます。


```matlab
pyenv % Use pyversion for MATLAB versions before R2019b
```

```text:Output
ans = 
  PythonEnvironment with properties:

          Version: "3.10"
       Executable: "C:\Users\ydebray\AppData\Local\WPy64-31040\python-3.10.4.amd64\python.exe"
          Library: "C:\Users\ydebray\AppData\Local\WPy64-31040\python-3.10.4.amd64\python310.dll"
             Home: "C:\Users\ydebray\AppData\Local\WPy64-31040\python-3.10.4.amd64"
           Status: NotLoaded
    ExecutionMode: OutOfProcess

```

## 2.1.2. MATLAB から Python のユーザー定義関数を呼び出す: Call Python user-defined functions from MATLAB

では、同僚のお天気モジュールの使い方をみてみましょう。まずは、今日のデータを取得するところから始めます。weather モジュール の [get_current_weather](https://github.com/hgorr/weather-matlab-python/blob/c8985b96b4c4a64b283573a5276d25f33f311bcc/weather.py#L16) 関数は、現在の天気を Json 形式で取得します。そして、[parse_current_json](https://github.com/hgorr/weather-matlab-python/blob/c8985b96b4c4a64b283573a5276d25f33f311bcc/weather.py#L42) 関数はそのデータを python dictionary として返します。



```matlab
jsonData = py.weather.get_current_weather("London","UK",appid,api='samples')
```


```text:Output
jsonData = 
  Python dict with no properties.

    {'coord': {'lon': -0.13, 'lat': 51.51}, 'weather': [{'id': 300, 'main': 'Drizzle', 'description': 'light intensity drizzle', 'icon': '09d'}], 'base': 'stations', 'main': {'temp': 280.32, 'pressure': 1012, 'humidity': 81, 'temp_min': 279.15, 'temp_max': 281.15}, 'visibility': 10000, 'wind': {'speed': 4.1, 'deg': 80}, 'clouds': {'all': 90}, 'dt': 1485789600, 'sys': {'type': 1, 'id': 5091, 'message': 0.0103, 'country': 'GB', 'sunrise': 1485762037, 'sunset': 1485794875}, 'id': 2643743, 'name': 'London', 'cod': 200}

```


```matlab
weatherData = py.weather.parse_current_json(jsonData)
```


```text:Output
weatherData = 
  Python dict with no properties.

    {'temp': 280.32, 'pressure': 1012, 'humidity': 81, 'temp_min': 279.15, 'temp_max': 281.15, 'speed': 4.1, 'deg': 80, 'lon': -0.13, 'lat': 51.51, 'city': 'London', 'current_time': '2023-03-15 16:04:38.427888'}

```

## 2.1.3. Python データを MATLAB データに変換する: Convert Python data to MATLAB data

[Python dictionary](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) を MATLAB の[構造体](https://www.mathworks.com/help/matlab/ref/struct.html) に変換してみましょう。


```matlab
data = struct(weatherData)
```


```text:Output
data = 
            temp: 280.3200
        pressure: [1x1 py.int]
        humidity: [1x1 py.int]
        temp_min: 279.1500
        temp_max: 281.1500
           speed: 4.1000
             deg: [1x1 py.int]
             lon: -0.1300
             lat: 51.5100
            city: [1x6 py.str]
    current_time: [1x26 py.str]

```

ほとんどのデータは自動的に変換されます。ただ、いくつかのフィールドは、明らかな等価物を見つけることができませんでした。

   -  `pressure` \& `humidity` remain as a `py.int` object in MATLAB. 
   -  `city` and `current_time `remain as a `py.str` object in MATLAB. 

[double](https://www.mathworks.com/help/matlab/ref/double.html), [string](https://www.mathworks.com/help/matlab/characters-and-strings.html)
, [datetime](https://www.mathworks.com/help/matlab/date-and-time-operations.html) などのMATLAB 標準関数を使用して明示的に変換することができます。


```matlab
data.pressure = double(data.pressure);
data.humidity = double(data.humidity);
data.deg = double(data.deg);
data.city = string(data.city);
data.current_time = datetime(string(data.current_time))
```


```text:Output
data = 
            temp: 280.3200
        pressure: 1012
        humidity: 81
        temp_min: 279.1500
        temp_max: 281.1500
           speed: 4.1000
             deg: 80
             lon: -0.1300
             lat: 51.5100
            city: "London"
    current_time: 15-Mar-2023 16:04:38

```

## 2.1.4. Python のリストを MATLAB の行列に変換する: Convert Python lists to MATLAB matrices

では、今後数日間の天気予報を返す [get_forecast](https://github.com/hgorr/weather-matlab-python/blob/c8985b96b4c4a64b283573a5276d25f33f311bcc/weather.py#L67) 関数を呼び出してみましょう。構造体のフィールドは [Python list](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists) として返されることがわかります。


```matlab
jsonData = py.weather.get_forecast('Muenchen','DE',appid,api='samples');
forecastData = py.weather.parse_forecast_json(jsonData);  
forecast = struct(forecastData)
```


```text:Output
forecast = 
    current_time: [1x36 py.list]
            temp: [1x36 py.list]
             deg: [1x36 py.list]
           speed: [1x36 py.list]
        humidity: [1x36 py.list]
        pressure: [1x36 py.list]

```

数値データのみを含むリストの場合は、doubleに変換することができます（MATLAB R2022a以降）。


```matlab
forecast.temp = double(forecast.temp) - 273.15; % from Kelvin to Celsius
forecast.temp
```

```text:Output
ans = 1x36    
   13.5200   12.5100    3.9000   -0.3700    0.1910    2.4180    3.3280    3.5200    5.1030    3.3050    2.4890    2.3090    1.8850    1.8150    1.4120    2.4980    4.7770    5.2170    0.6470   -1.9110   -3.5970   -4.9520   -5.8550   -0.1940    4.2720    4.8340   -0.6910   -3.6770   -4.3570   -5.0440   -5.4950    0.6000    6.1520    6.1930    1.2930   -0.7260

```

テキストを含むリストは文字列に変換でき、さらに datetime のような特定のデータ型に処理することができます。


```matlab
forecast.current_time = string(forecast.current_time);
forecast.current_time = datetime(forecast.current_time);
forecast.current_time
```


```text:Output
ans = 1x36 datetime    
16-Feb-2017 12:00:0016-Feb-2017 15:00:0016-Feb-2017 18:00:0016-Feb-2017 21:00:0017-Feb-2017 00:00:0017-Feb-2017 03:00:0017-Feb-2017 06:00:0017-Feb-2017 09:00:0017-Feb-2017 12:00:0017-Feb-2017 15:00:0017-Feb-2017 18:00:0017-Feb-2017 21:00:0018-Feb-2017 00:00:0018-Feb-2017 03:00:0018-Feb-2017 06:00:0018-Feb-2017 09:00:0018-Feb-2017 12:00:0018-Feb-2017 15:00:0018-Feb-2017 18:00:0018-Feb-2017 21:00:0019-Feb-2017 00:00:0019-Feb-2017 03:00:0019-Feb-2017 06:00:0019-Feb-2017 09:00:0019-Feb-2017 12:00:0019-Feb-2017 15:00:0019-Feb-2017 18:00:0019-Feb-2017 21:00:0020-Feb-2017 00:00:0020-Feb-2017 03:00:00

```

Python と MATLAB 間のデータのマッピングについてはこちらをご覧ください（4.7節）。


## 2.1.5. MATLAB に取り込まれた Python のデータを可視化する: Explore graphically the Python data imported in MATLAB

```matlab
plot(forecast.current_time,forecast.temp)
xtickangle(45)
xlabel('Date')
ylabel('Temperature')
```

<img src="./media/image11.png" alt="Chart, line chart" />

## 2.1.6. MATLAB で機械学習モデルを呼び出す: Call a Machine Learning model in MATLAB

ここで、過去のデータを使って、気象条件から空気の質の予測を返す機械学習モデルを作成したとします。 Python を使う同僚は、彼女の Python コードで私のモデルを利用したいと考えています。

まず、空気の質の予測がどのように機能するかを見てみましょう。 3つのステップがあります。

   - .matファイルからモデルを読み込む 
   - openweathermap.org の現在の気象データを、モデルが期待する形式に変換する。
   - モデルの predict メソッドを呼び出して、その日に予想される空気質を取得する 

```matlab
load airQualModel.mat model
testData = prepData(data);
airQuality = predict(model,testData)
```


```text:Output
airQuality = 
     Good 

```

これを同僚に渡すために、これらのステップを [predictAirQuality](https://github.com/hgorr/weather-matlab-python/blob/main/predictAirQual.m) という一つの関数にまとめることにします。

```matlab
function airQual = predictAirQual(data)
% PREDICTAIRQUAL Predict air quality, based on machine learning model
%
%#function CompactClassificationEnsemble

% Convert data types  
currentData = prepData(data);

% Load model
mdl = load("airQualModel.mat");
model = mdl.model;

% Determine air quality
airQual = predict(model,currentData);

% Convert data type for use in Python
airQual = char(airQual);

end
```

この関数は、モデルをロードし、データを変換し、モデルの predict メソッドを呼び出すという、上記と同じ3つのステップを実行します。 

しかし、もう一つやらなければならないことがあります。 モデルは MATLAB のカテゴリ値を返しますが、Python ではこれに直接相当するものがないので、文字配列に変換しています。

さて、大気質予測モデルを使用する MATLAB 関数ができたので、Python でどのように使用するか見てみましょう。


## 2.2. Python から MATLAB を呼び出す: Call MATLAB from Python

ここでは、Jupyter ノートブックを使って Python から MATLAB を呼び出すデモを紹介します。

最初のステップは、engine API を使用して、Python が通信するためにバックグラウンドで動作する MATLAB を起動することです（ここでは、すでにインストールされていると仮定します - そうでなければ[section 3.8](3_Set-up_MATLAB_and_Python.md)を確認してください）。

```python
>>> import matlab.engine
>>> m = matlab.engine.start_matlab()
```

MATLABが起動したら、パス上の任意の MATLAB 関数を呼び出すことができます。

```python
>>> m.sqrt(42.0)
6.48074069840786
```

txt ファイルから key にアクセスする必要があります。

```python
>>> with open("accessKey.txt") as f:
...   apikey = f.read()
```


MATLAB で行ったように、weather モジュールの get_current_weather 関数と parse_current_json 関数を使用して、現在の気象条件を取得することができます。

```python
>>> import weather
>>> json_data = weather.get_current_weather("Boston","US",apikey)
>>> data = weather.parse_current_json(json_data)
>>> data
{'temp': 62.64, 'feels_like': 61.9, 'temp_min': 58.57, 'temp_max': 65.08, 'pressure': 1018, 'humidity': 70, 'speed': 15.01, 'deg': 335, 'gust': 32.01, 'lon': -71.0598, 'lat': 42.3584, 'city': 'Boston', 'current_time': '2022-05-23 11:28:54.833306'}
```

そして、MATLAB の関数 predictAirQuality を呼び出して、予測結果を得ることができます。


```python
>>> aq = m.predictAirQuality(data)
>>> aq
Good
```

最後のステップは、ノートブックの冒頭でエンジン API によって起動された MATLAB のシャットダウンです。

```python
>>> m.exit()
```

しかし、Python を使う同僚は MATLAB にアクセスがないかもしれません。次の 2 つのセクションでは、このユースケースを対象とします。


## 2.3. MATLAB 関数群から Python パッケージを生成する: Generate a Python package from a set of MATLAB functions

そのためには、[MATLAB Compiler SDK](https://www.mathworks.com/help/compiler_sdk/) という専用の Toolbox を使用する必要があります。アプリリボンのライブラリコンパイラを選択するか、コマンドウィンドウに`libraryCompiler` と入力してください。

<img src="./media/image17.png" alt="Graphical user interface Library Compiler" />

MATLAB 関数を選択し、Python 関数に変換します。依存関係が自動的に Python パッケージに追加されます（この場合、大気質モデル、都市のリスト、および前処理関数）。

これにより、必要なファイルがパッケージ化され、Python の手順が書かれた *setup.py* と *readme.txt* ファイルが作成されます。生成されたパッケージをどのようにセットアップするかについては [section 6.1](#set-up-of-the-generated-python-package) を読んでください。

次に、パッケージをインポートして初期化。このように関数を呼び出すことができます。


```python
>>> import AirQual
>>> aq = AirQual.initialize()
>>> result = aq.predictAirQual()
```

終了したら、プロセスを終了して、一連の作業を終わらせます。

```python
>>> aq.terminate()
```

さらに一歩進んで、MATLABの機能を共有し、Webサービスとして利用することも可能です（一度に多くのユーザーがアクセスできる可能性があります）。この場合、[MATLAB Production Server](https://www.mathworks.com/help/mps/index.html) を負荷分散に使用し、MATLAB コードには [RESTful
API](https://www.mathworks.com/help/mps/restful-api-and-json.html) や [Python client](https://www.mathworks.com/help/mps/python/create-a-matlab-production-server-python-client.html) を介してアクセスすることが可能です。

