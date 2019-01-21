<h1 align="center"> saviorlv/yii2-bd-aipspeech </h1>

<p align="center"> 基于百度AI 的语音合成、转换的 Yii2 sdk.</p>


## 安装

```shell
$ composer require saviorlv/yii2-bd-aipspeech -vvv
```

## 配置

```php
  // 配置文件里修改
  'components' => [
        ......
        'aipSpeech' => [
            'class' => 'Saviorlv\Baidu\BdSpeech',
            'app_id' => 'xxxxxx', // 百度语音 App ID
            'api_key' => 'xxxxxxx', // 百度语音 API Key
            'secret_key' => 'xxxxxx', // 百度语音 Secret Key
            'path' => Yii::getAlias('@tmp'.'/audios/') //可以不填写 默认在 runtime
        ],
        ......
    ],

```

## 使用

> 1. 语音转换

```php
//请求
 $aipSpeech = Yii::$app->get('aipSpeech');
  $file = Yii::getAlias('@tmp'.'/audios/').'16k.pcm';
  $x = $aipSpeech->recognize($file,'');
   var_dump($x);
```

```php
//响应
[
  'success' =>true,
  'msg' => '语音识别成功',
  'data' =>[
    ......
  ]
]
//or
[
  'success' =>false,
  'msg' => '语音文件路径错误',
]
```

> 2. 语音合成


```php
//请求
 $aipSpeech = Yii::$app->get('aipSpeech');
 $x = $aipSpeech->combine('您好，世界');
  var_dump($x);
```

```php
//响应
[
  'success' =>true,
  'msg' => '语音合成成功',
  'data' =>'/webwww/yii2-bd/tmp/audios/5c4575feeb70d.mp3'
]
//or
[
  'success' =>false,
  'msg' => '语音合成失败',
]
```

## 说明

> 语音识别参数说明


### 用法
```php
/**
     * 语音识别
     *
     * @param $filePath string 语音文件本地路径,优先使用此项
     * @param $url string 语音文件URL路径
     * @param $userID string 用户唯一标识
     * @param $format string 语音文件格式 ['pcm', 'wav', 'opus', 'speex', 'amr']
     * @param $rate integer 采样率 [8000, 16000]
     * @param $dev_pid int 语音语言 [1536,1537,1737,1637,1837,1936]
     * @return array
     */
    public function recognize($filePath, $url, $format = 'wav', $dev_pid = 1536, $userID = null, $rate = 16000)
    {}

```

###  参数
 
<table>
    <thead>
        <tr>
            <th style="text-align:left">参数</th>
            <th style="text-align:left">类型</th>
            <th style="text-align:left">描述</th>
            <th style="text-align:left">是否必须</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align:left">$filePath</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">建立包含语音内容的本地, 语音文件的格式，pcm 或者 wav 或者 amr。不区分大小写</td>
            <td style="text-align:left">是（url 二选一）</td>
        </tr>
        <tr>
            <td style="text-align:left">$url</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">建立包含语音内容的url, 语音文件的格式，pcm 或者 wav 或者 amr。不区分大小写</td>
            <td style="text-align:left"> 是（filePath 二选一）</td>
        </tr>
        <tr>
            <td style="text-align:left">format</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">语音文件的格式，pcm 或者 wav 或者 amr。不区分大小写。推荐pcm文件</td>
            <td style="text-align:left">是</td>
        </tr>
        <tr>
            <td style="text-align:left">rate</td>
            <td style="text-align:left">int</td>
            <td style="text-align:left">采样率，16000，固定值</td>
            <td style="text-align:left">是</td>
        </tr>
        <tr>
            <td style="text-align:left">userId</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">用户唯一标识，用来区分用户，填写机器 MAC 地址或 IMEI 码，长度为60以内</td>
            <td style="text-align:left">否</td>
        </tr>
        <tr>
            <td style="text-align:left">dev_pid</td>
            <td style="text-align:left">Int</td>
            <td style="text-align:left">不填写lan参数生效，都不填写，默认1537（普通话 输入法模型），dev_pid参数见本节开头的表格</td>
            <td style="text-align:left">否</td>
        </tr>
    </tbody>
</table>


> 语音合成参数说明

### 用法

```php
/**
     * 语音合成
     *
     * @param $text string 合成的文本
     * @param $userID string 用户唯一标识
     * @param $lan string 语音 ['zh']
     * @param $speed integer 语速，取值0-9，默认为5中语速
     * @param $pitch integer 音调，取值0-9，默认为5中语调
     * @param $volume integer 音量，取值0-15，默认为5中音量
     * @param $person integer 发音人选择, 0为女声，1为男声，3为情感合成-度逍遥，4为情感合成-度丫丫，默认为普通女
     * @param $fileName string 存储文件路径名称
     * @return array
     */
    public function combine($text, $userID = null, $lan = 'zh', $speed = 5, $pitch = 5, $volume = 5, $person = 0, $fileName = null){}
```

### 参数
<table>
    <thead>
        <tr>
            <th style="text-align:left">参数</th>
            <th style="text-align:left">类型</th>
            <th style="text-align:left">描述</th>
            <th style="text-align:left">是否必须</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align:left">tex</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">合成的文本，使用UTF-8编码，<br>请注意文本长度必须小于1024字节</td>
            <td style="text-align:left">是</td>
        </tr>
        <tr>
            <td style="text-align:left">userID</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">用户唯一标识，用来区分用户，<br>填写机器 MAC 地址或 IMEI 码，长度为60以内</td>
            <td style="text-align:left">否</td>
        </tr>
        <tr>
            <td style="text-align:left">speed</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">语速，取值0-9，默认为5中语速</td>
            <td style="text-align:left">否</td>
        </tr>
        <tr>
            <td style="text-align:left">pitch</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">音调，取值0-9，默认为5中语调</td>
            <td style="text-align:left">否</td>
        </tr>
        <tr>
            <td style="text-align:left">volume</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">音量，取值0-15，默认为5中音量</td>
            <td style="text-align:left">否</td>
        </tr>
        <tr>
            <td style="text-align:left">person</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">发音人选择, 0为女声，1为男声，<br>3为情感合成-度逍遥，4为情感合成-度丫丫，默认为普通女</td>
            <td style="text-align:left">否</td>
        </tr>
        <tr>
            <td style="text-align:left">fileName</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">合成文件名称</td>
            <td style="text-align:left">否</td>
        </tr>
        <tr>
            <td style="text-align:left">lan</td>
            <td style="text-align:left">String</td>
            <td style="text-align:left">合成语音的语言  默认 zh</td>
            <td style="text-align:left">否</td>
        </tr>
    </tbody>
</table>

## 参考文件

[百度语音](http://ai.baidu.com/docs#/ASR-Online-PHP-SDK/top), 一定要先看文档

> 感谢

[e-yunduan/yii2-aip-speech](https://github.com/e-yunduan/yii2-aip-speech)

## License

MIT