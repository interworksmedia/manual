---
layout: default
title: VAST Guide
---

### Changelog

| Version | Date | Description |
| :-: | :-: | :- |
| 1.0 | 2018-04-01 | Release |
| 1.1 | 2018-04-15 | Add `<Icon>` Element |
| 1.2 | 2018-05-31 | Modify Parameters |
| 1.3 | 2018-06-04 | Modify Parameters |
| 1.4 | 2018-09-04 | Modify Category parameter |
| 1.5 | 2018-10-22 | Modify Resolution parameter |
| 1.6 | 2018-11-27 | Add `<Extensions>` Element |
| 1.7 | 2018-12-18 | Remove `<Extensions>` Element, Add `<Icons>` Element |

### Table of Contents

1. [Request](#request)
2. [Response](#response)



### Request

**URL**

```html
https://tv.interworksmedia.co.kr/?[parameters]
```

**Parameters**

> fmt=json is not implementation.
>

| Key | Description | Required | Data Type | Value |
| - | - | :-: | :-: | - |
| fmt | 응답포맷 | N | String | xml, json (default: **xml**) |
| cat | 앱 카테고리 | **Y** | String | _ 기호로 중복 가능 <br />(ex. game_sns_shopping ) |
| mid | 미디어ID | **Y** | String | 인터웍스미디어에서 발급한 키 |
| kind | 광고노출형태 | **Y** | Number | 1: In-stream, 2: Out-stream |
| version | VAST 버전 | N | Number | 3, 4 (Default: **3**) |
| skipmin | SKIP 가능 최소 시간(초) | **Y** | Number | 0, 5, 15 |
| skipmax | SKIP 가능 최대 시간(초) | **Y** | Number | 0, 5, 15 |
| duration | 수용가능한 최대 영상길이(초)  | **N** | Number | 비 스킵상품에만 적용. skipmin 또는 skipmax에 0이 포함될 때 Requierd |
| id | 디바이스 ADID | N | String | Android: ADID, iOS: IDFA |
| age | 나이 | N | Number | [Age code 참조](#age-code) |
| gender | 성별 | N | Number | 1: 남성, 2: 여성 |
| resolution | 영상 해상도 | **Y** | Number | 1: 360p(640x360), 2: 720p(1280x720), <br />3: 270p(480x270) |


#### Age code

| 구간 | 값 |
| :-: | :-: |
| 9세 이하 | 0 |
| 10 - 19세 | 1 |
| 20 - 29세 | 2 |
| 30 - 39세 | 3 |
| 40 - 49세 | 4 |
| 50 - 59세 | 5 |
| 60세 이상 | 6 |

**Requese URL Example**

```bash
# Test media id = 000000
https://tv.interworksmedia.co.kr/?cat=game&mid=000000&kind=2&skipmin=5&skipmax=15&resolution=1
```

**[⬆ back to top](#table-of-contents)**



### Response

**Status code**

| Code | Description |
| :-: | :- |
| 200 | OK |
| 204 | No Ad. |
| 500 | Internal server error |

**VAST XML Schema**

| Element | Attributes | Required |
| - | - | :-: |
| **VAST** | version | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;/Error |  |N|
| **VAST/Ad** | id, sequence | Y |
| **VAST/Ad/InLine** || Y** |
| &nbsp;&nbsp;&nbsp;&nbsp;/AdSystem | version | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;/AdTitle | | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;/Impression | id | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;**/Creatives** | | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/Creatives | id, sequence | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/Creative | | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**/Linear** | skipoffset | Y** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/Duration | | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/MediaFiles | | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/MediaFile | delivery, type, width, height | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/TrackingEvents | | N |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/Tracking | event, offset | N |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/VideoClicks | | N |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/ClickThrough | id | N |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/ClickTracking | id | N |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/Icons | | N |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/Icon | program, width, height, xPosition, yPosition, duration, offset | Y** |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/StaticResource | creativeType | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/IconClicks | | N |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/IconClickThrough | | N |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/IconClickTracking | | N |
| &nbsp;&nbsp;&nbsp;&nbsp;/Extensions | | Y |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;/Extension | | Y |

> ** 나열된 요소 중 하나가 필요하거나 상위 요소가 사용되는지 여부에 따라 다름



**Response Examaple**

> VAST 3.0 XML Document example

```xml
<VAST version="3.0">
    <Ad id="TST1810_CVPIP" sequence="1">
        <InLine>
            <AdSystem><![CDATA[ introTV ]]></AdSystem>
            <AdTitle />
            <Impression id="1876180063"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=impression&event_time=0&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Impression>
            <Creatives>
                <Creative id="info_A.html" sequence="1">
                    <Linear skipoffset="00:00:15">
                        <Duration>00:00:30</Duration>
                        <Icons>
                            <Icon program="click" width="90" height="90" xPosition="left" yPosition="bottom" offset="00:00:00" duration="00:00:30">
                                <IconClicks>
                                    <IconClickThrough><![CDATA[https://interface.interworksmedia.co.kr/click?q=2%5E118622%5E1546845890522%5E%2F%2Fclick.iwmedia.co.kr%2Fwww.iwmedia.co.kr%2Fiwm_news%2FL26%2F1876180063%2Fx77%2FService_ads_080621%2FTST1810_CVPIP%2Fclick_etc_A.html%2F774b675546567759557745414168624c%3F_1234]]></IconClickThrough>
                                    <IconClickTracking><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=click&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></IconClickTracking>
                                </IconClicks>
                                <StaticResource creativeType="image/png"><![CDATA[ https://cdn.interworksmedia.co.kr/vast/CLICK.png ]]></StaticResource>
                            </Icon>
                            <Icon program="skip" width="210" height="70" xPosition="right" yPosition="bottom" offset="00:00:15" duration="00:00:30">
                                <IconClicks>
                                    <IconClickTracking><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=skip&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></IconClickTracking>
                                </IconClicks>
                                <StaticResource creativeType="image/png"><![CDATA[ https://cdn.interworksmedia.co.kr/vast/SKIP.png ]]></StaticResource>
                            </Icon>
                            <Icon program="skipwait" width="240" height="60" xPosition="right" yPosition="bottom" offset="00:00:00" duration="00:00:15">
                                <StaticResource creativeType="image/png"><![CDATA[https://cdn.interworksmedia.co.kr/vast/SKIPWAIT.png]]></StaticResource>
                            </Icon>
                        </Icons>
                        <TrackingEvents>
                            <Tracking event="start"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=start&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="complete"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=complete&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="skip"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=skip&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:01"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=1&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:02"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=2&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:03"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=3&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:04"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=4&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:05"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=5&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:06"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=6&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:07"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=7&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:08"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=8&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:09"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=9&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:10"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=10&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:11"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=11&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:12"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=12&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:13"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=13&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:14"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=14&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:15"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=15&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:16"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=16&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:17"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=17&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:18"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=18&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:19"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=19&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:20"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=20&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:21"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=21&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:22"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=22&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:23"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=23&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:24"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=24&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:25"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=25&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:26"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=26&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:27"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=27&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:28"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=28&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:29"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=29&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="progress" offset="00:00:30"><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=progress&event_time=30&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></Tracking>
                            <Tracking event="start"><![CDATA[https://ds.interworksmedia.co.kr/RealMedia/ads/adstream_jx.ads/www.iwmedia.co.kr/iwm_news@x74?TST1810_A]]></Tracking>
                            <Tracking event="progress" offset="00:00:15"><![CDATA[https://interface.interworksmedia.co.kr/click?q=2%5E118622%5E1546845890522%5E%2F%2Fclick.iwmedia.co.kr%2Fwww.iwmedia.co.kr%2Fiwm_news%2FL26%2F1876180063%2Fx78%2FService_ads_080621%2FTST1810_CVPIP%2Fimp_A.html%2F774b675546567759557745414168624c%3F_1234]]></Tracking>
                        </TrackingEvents>
                        <VideoClicks>
                            <ClickThrough><![CDATA[https://interface.interworksmedia.co.kr/click?q=2%5E118622%5E1546845890522%5E%2F%2Fclick.iwmedia.co.kr%2Fwww.iwmedia.co.kr%2Fiwm_news%2FL26%2F1876180063%2Fx77%2FService_ads_080621%2FTST1810_CVPIP%2Fclick_etc_A.html%2F774b675546567759557745414168624c%3F_1234]]></ClickThrough>
                            <ClickTracking><![CDATA[https://tv.interworksmedia.co.kr/tracking?event=click&play_id=e2efbc88-4697-44f1-ba29-2007b6cf0098&kind=2&mid=000000&category=_1234&campaign_id=TST1810_CVPIP&creative_idx=A&duration=30&site=www.iwmedia.co.kr&page=iwm_news&age=&gender=&id=]]></ClickTracking>
                        </VideoClicks>
                        <MediaFiles>
                            <MediaFile delivery="progressive" type="video/mp4" width="640" height="360"><![CDATA[https://cdn.interworksmedia.co.kr/PID0715/NK/A/640_360.mp4]]></MediaFile>
                        </MediaFiles>
                    </Linear>
                </Creative>
            </Creatives>
            <Extensions />
        </InLine>
    </Ad>
</VAST>
```



### Icons

인터웍스미디어의 커스텀 스킵, 클릭 버튼태그

클릭은 좌측 하단, 스킵은 우측 하단에 사용



**Icon type**

1. Click: click버튼
2. Skip: skip버튼
3. Skipwait : skip버튼 이전에 보여지는 버튼



**화면 샘플**

![sample_01](./resources/vast/sample_01.jpg)



![sample_02](./resources/vast/sample_02.jpg)





**[⬆ back to top](#table-of-contents)**

