# globalping - Globalping Ping Test

[Globalping](https://globalping.io) API로 특정 국가/도시의 프로브에서 ping을 실행하는 Python 스크립트입니다.

## 목적

특정 지역에서 대상에 닿는지, 지연이 얼마나 되는지를 그 지역의 프로브로 직접 측정합니다. 지역별 접속 문제가 의심될 때 "그 지역에서는 어떻게 보이는가"를 확인하는 데 씁니다. 기본 위치는 러시아(RU)입니다.

## Features

- **Country/City Filter**: 국가 코드와 도시 이름으로 프로브 위치 지정
- **Multi Probe**: 프로브 수 지정 (기본 3개), 프로브마다 ping 5회
- **Table Output**: 프로브별 Min / Avg / Max / Loss를 표로 출력
- **No Auth Required**: 공개 API라 토큰 없이 실행 (한도를 늘리려면 `--token` 지정)

## Requirements

- Python 3.6+
- requests

```bash
pip install requests
```

## Usage

```bash
python globalping_ping.py <target> [--country CODE] [--city NAME] [--probes N] [--token TOKEN]
```

| 옵션 | 설명 | 기본값 |
|------|------|--------|
| `target` | 대상 호스트명 또는 IP | (필수) |
| `--country` | 프로브 국가 코드 | `RU` |
| `--city` | 프로브 도시 이름 | 없음 |
| `--probes` | 프로브 수 | `3` |
| `--token` | Globalping API 토큰 (한도 상향용) | 없음 |

```bash
# 러시아 프로브 3개에서 ping
python globalping_ping.py example.com

# 한국 프로브
python globalping_ping.py example.com --country KR

# 미국 뉴욕 프로브 5개
python globalping_ping.py example.com --country US --city "New York" --probes 5
```

## Output

```
Probe                          Min      Avg      Max     Loss
---------------------------------------------------------------
RU / Moscow / AS00000          xx.x     xx.x     xx.x     0.0%
```

프로브가 응답하지 않으면 해당 행은 `N/A`로 표시됩니다. 한도 초과(429)나 잘못된 위치 조건(422)이면 API 메시지를 출력하고 종료합니다.
