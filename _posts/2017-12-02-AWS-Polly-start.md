---
layout: post
title: 【AWS-Polly】start
date: 2017-12-02 16:30
categories: aws polly
---

# SYNOPSIS

1. 了解 AWS Polly 基礎
2. 執行基本 cli 測試

<!--more-->

# 可設定的
- 各類型不同地區的語言
- 聲音類型 : 有男女、老少不同的語調
	- 客製化發音
- 輸入的文字格式 : Plain text, SSML(speech synthesis markup language)
- Lexicons : 客製化詞庫與發音

# 下載的格式有以下幾種(主要看播放器支援哪種格式的播放吧)
- File format : MP3, OGG, PCM, Speech Marks
	- Speech Marks Types : Viseme, Word, Sentence, SSML(speech synthesis markup language)
- Rate : 22050Hz, 16000Hz, 8000Hz, 可自訂也可系統預設

# 使用方式
- AWS Console
- AWS SDKS
- AWS CLI : 這邊使用 cli 當做範例
- API reference : `http://docs.aws.amazon.com/polly/latest/dg/API_Reference.html`

# AWS CLI

1. 必須先安裝且設定 AWS CLI : `http://docs.aws.amazon.com/polly/latest/dg/setup-aws-cli.html`
2. 這邊的範例使用 `aws polly synthesize-speech`
	1. 執行指令
	2. 取得下載的語音檔案
	3. 進行播放

## POLLOY 可用的指令

```
# 輸入 aws polly help
delete-lexicon
describe-voices
get-lexicon
help
list-lexicons
put-lexicon
synthesize-speech
```

## POLLY : SynthesizeSpeech

> 如果你在 aws-cli 中設定的地區不支援 polly, 則你必須使用 `--region polly-supported-aws-region` 來指定使用 polly 服務的地區
> 如 : 你用了某個地區的服務，但該地區並不支援 polly，則若你要使用 polly，則必須額外指定地區，來讓你的服務正常
> `aws polly synthesize-speech help`

```
# 輸入, 以下為必要選項
aws polly synthesize-speech \
    --output-format mp3 \
    --voice-id Joanna \
    --text 'Hello, my name is Joanna. I learned about the W3C on 10/3 of last year.' \
    hello.mp3
    
# 輸出
{
    "ContentType": "audio/mpeg", 
    "RequestCharacters": "71"
}
```
執行指令後，會取得 hello.mp3，在你的應用程式進行播放


## POLLY : Describe Voices

> 取得所有可以用的聲音設定，會有不同的語調與速度，可在 aws polly console 測試

```
# 輸入
aws polly describe-voices
# 輸出
{
    "Voices": [
        {
            "Gender": "Female",
            "Name": "Salli",
            "LanguageName": "US English",
            "Id": "Salli",
            "LanguageCode": "en-US"
        },
        {
            "Gender": "Female",
            "Name": "Joanna",
            "LanguageName": "US English",
            "Id": "Kendra",
            "LanguageCode": "en-US"
        }
        ...
    ]
}
```

- 除此之外，`describe-voice` 還可取得其他的設定，如 `--language-code` : `aws polly describe-voices --language-code pt-BR(Brazilian Portuguese. 巴西葡萄牙語)`
- 使用 `aws polly describe-voices help` 取得更多的幫助
