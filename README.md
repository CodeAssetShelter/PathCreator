# Path Creator (Unity)

> **Bezier/Linear paths, arc‑length (uniform speed), speed profiles, runtime branching & blending — with handy editor gizmos.**  
> **유니티용 경로 시스템:** 베지어/직선 경로, 등속(호길이 보정), 속도 커브, 런타임 분기/블렌드, 편집기 기즈모 제공.

---

## Table of Contents / 목차

- [Overview](#overview)
- [Features](#features)
- [Package Structure](#package-structure)
- [Requirements & Install](#requirements--install)
- [Quick Start](#quick-start)
- [Core Scripts](#core-scripts)
- [Advanced Usage](#advanced-usage)
- [Editor Workflow Tips](#editor-workflow-tips)
- [Troubleshooting / FAQ](#troubleshooting--faq)
- [Recent Additions / 최근 추가 기능](#recent-additions--최근-추가-기능)
- [Roadmap](#roadmap)
- [License](#license)
- [Contact](#contact)

---

## Overview

**EN**  
This repository provides a lightweight yet extendable path system for Unity. You can draw paths using points and choose between linear or Bezier segments. Objects can follow those paths with optional uniform speed, custom speed curves, looping, reversing, and runtime path switching.

**KR**  
이 저장소는 Unity에서 간단하지만 확장 가능한 경로 시스템을 제공합니다. 포인트를 배치해 직선/베지어 세그먼트를 만들고, 오브젝트를 해당 경로로 이동시킬 수 있습니다. 등속 이동, 속도 커브, 루프/역방향, 런타임 분기 전환 등 다양한 기능을 지원합니다.

---

## Features

**EN**

- Linear & Cubic Bezier segments  
- Arc‑length parameterization (uniform speed)  
- Global speed profile via `AnimationCurve`  
- Loop / reverse / manual start–stop controls  
- Auto-start on enable (`autoStart`)  
- Runtime path switching (keep-progress, warp, smooth blend)  
- Sync speed curve keys based on points or path distance  
- Full 3D support (Vector3-based points & handles)  
- Scene gizmos with pivot indices & draggable handles

**KR**

- 직선 & 3차 베지어 세그먼트 지원  
- 호길이 보정(Arc‑length) 기반 등속 이동  
- `AnimationCurve` 로 전체 구간 속도 프로필 제어  
- 루프 / 역방향 / 수동 시작·정지 기능  
- 오브젝트 활성화 시 자동 시작 옵션(`autoStart`)  
- 런타임 경로 전환(진행률 유지, 워프, 스무스 블렌드)  
- 포인트 수/거리 비율에 맞춘 속도 커브 키 자동 동기화  
- 3D 완전 지원(Vector3 포인트 & 핸들)  
- 씬 기즈모: 피봇 인덱스, 드래그 가능한 핸들

---

## Package Structure

```text
Assets/
  Utils/PathCreator/
    PathCreator.cs
    PathFollower.cs
    PathCreatorEditor.cs
    Sample.unity
```

**EN**: Drop this folder into your project. `Sample.unity` shows basic setups.  
**KR**: 이 폴더를 그대로 프로젝트에 넣으면 됩니다. `Sample.unity` 씬에서 기본 사용 예시를 확인할 수 있습니다.

---

## Requirements & Install

**EN**

* Unity **2020.3 LTS** or newer (tested to 6000.0.36f1).  
* Just drop the `PathCreator` folder into your project, or import the `.unitypackage`.

**KR**

* Unity **2020.3 LTS** 이상 권장 (6000.0.36f1 버전에서 테스트 완료).
* `PathCreator` 폴더를 프로젝트에 복사하거나 `.unitypackage`를 임포트하세요.

---

## Quick Start

### EN

1. **Create Root** GameObject (e.g., `PathUnit`).  
2. Add **PathCreator** to a child at local. Arrange points & set segment type (Line/Bezier).  
3. Add **PathFollower** to another child (the mover). Assign `path` reference to the PathCreator.  
4. Set `duration`, `offset`, and toggle `useUniform` as needed.  
5. Press **Play**.  
   * If `autoStart` is true (default), it starts automatically when enabled.  
   * Otherwise call `StartFollow()`.

### KR

1. 루트 GameObject(예: `PathUnit`)를 만듭니다.  
2. 자식 오브젝트에 **PathCreator**를 붙이고 로컬에 둡니다. 포인트를 배치하고 세그먼트 타입(Line/Bezier)을 설정합니다.  
3. 다른 자식(이동체)에 **PathFollower**를 붙인 뒤 `path`에 PathCreator를 넣습니다.  
4. `duration`, `offset`, `useUniform` 등을 설정합니다.  
5. **Play** 후 자동 시작을 원하면 `autoStart` 를 켭니다(기본값 true). 끄면 `StartFollow()` 를 호출하세요.

---

## Core Scripts

### PathCreator

**EN**

* Holds an array of `PathPoint` (position, handleIn/Out, segment type)  
* Builds arc‑length table for uniform speed  
* Gizmo drawing (lines, Bezier curves, pivot numbers)  
* `Evaluate(t)` and `EvaluateUniform(u)` return world positions

**KR**

* `PathPoint` 배열 보유(포지션, 핸들 in/out, 세그먼트 타입)  
* 등속 이동을 위한 호길이 테이블 생성  
* 기즈모로 직선/베지어, 피봇 번호를 씬에 표시  
* `Evaluate(t)`, `EvaluateUniform(u)` 로 월드 좌표 반환

### PathFollower

**EN**

* Drives a Transform along a path.  
* Options: `duration`, `offset`, `useUniform`, `loop`, `reverse`, `autoStart`.  
* Global speed profile with `speedCurve` (AnimationCurve).  
* Utility / Control methods:  
  * `StartFollow(float startOffset = -1f)`, `StopFollow()`  
  * `SwitchPath(PathCreator newPath)` — keep-progress branch change (Method A)  
  * `SyncSpeedCurveKeys()` (per-point even spacing)  

**KR**

* 경로를 따라 Transform을 이동시킵니다.  
* 옵션: `duration`, `offset`, `useUniform`, `loop`, `reverse`, `autoStart`.  
* `speedCurve`(AnimationCurve)로 전체 속도 프로필 제어.  
* 유틸/제어 메서드:  
  * `StartFollow(float startOffset = -1f)`, `StopFollow()`  
  * `SwitchPath(PathCreator newPath)` — 진행률 유지 방식 분기 전환
  * `SyncSpeedCurveKeys()` — 포인트 개수 기준 균등 키 생성  

---

## Advanced Usage

### Uniform Speed (Arc‑Length)

**EN**

* Set `useUniform = true` to make speed constant regardless of segment length.  
* PathCreator auto-rebuilds its arc table on edit (`autoRebuild`).

**KR**

* 세그먼트 길이에 상관없이 일정한 속도를 원하면 `useUniform = true`.  
* PathCreator는 편집 시 자동으로 호길이 테이블을 재계산합니다(`autoRebuild`).

### Speed Curve

**EN**

* `AnimationCurve speedCurve`: X = path ratio (0~1), Y = speed multiplier.  
* Example: boost from 0.3 to 0.5 by setting Y = 2.0 at those keys.  
* **Auto-generate keys:**  
  * `SyncSpeedCurveKeys()` – one key per PathCreator point (even spacing).  
  * `SyncSpeedCurveByArc()` – place keys by actual path distance ratio.

**KR**

* `AnimationCurve speedCurve`: X = 경로 비율(0~1), Y = 속도 배수.  
* 예: 0.3~0.5 구간에서 Y=2.0 으로 올리면 그 구간만 2배 가속.  
* **키 자동 생성:**  
  * `SyncSpeedCurveKeys()` – 포인트마다 균등 간격으로 키 생성.  
  * `SyncSpeedCurveByArc()` – 실제 경로 길이 비율에 맞게 키 위치 배치.

### Branching (Switching Paths)

**EN**

* Put multiple PathCreators under one root.  
* Call `SwitchPath(newPath)` to keep progress.  

**KR**

* 하나의 루트 아래 여러 PathCreator를 배치합니다.  
* `SwitchPath(newPath)`를 호출해 진행률을 유지하며 전환.
  
---

## Editor Workflow Tips

**EN**

* Enable the Scene Gizmo toggle to see cyan lines & magenta handles.  
* If pivot labels hide under gizmos, we draw them in GUI space with an offset.  
* Prevent null `PathPoint` entries by instantiating them in `OnValidate()`.

**KR**

* 씬 상단의 Gizmo 버튼을 켜면 cyan 선과 magenta 핸들을 볼 수 있습니다.  
* pivot 라벨이 가려지면 GUI 좌표로 그려 상단에 보이도록 처리했습니다.  
* `OnValidate()`에서 null `PathPoint`를 자동 생성해 직렬화 오류를 방지합니다.

---

## Troubleshooting / FAQ

**Q: My object doesn’t move. (오브젝트가 움직이지 않아요)**  
**EN**: Ensure `path` is assigned, `duration > 0`, and you called `StartFollow()` (or enabled auto start). Also check no other script overrides `transform.position`.  
**KR**: `path`가 비어있지 않은지, `duration > 0`인지, `StartFollow()`를 호출했는지(또는 `autoStart`가 켜져 있는지) 확인하세요. 다른 스크립트가 `transform.position`을 덮어쓰는지도 점검하세요.

**Q: Pivot inspector fields are blank. (피봇 인스펙터가 렌더링되지 않습니다)**  
**EN**: Remove `[Min]` from the array field; make sure `PathPoint` is serializable and not wrapped in `#if UNITY_EDITOR`.  
**KR**: 배열 필드에 `[Min]` 같은 수치용 Attribute를 붙이지 말고, `PathPoint`가 `[Serializable]`이며 `#if UNITY_EDITOR` 밖에 선언되어 있는지 확인하세요.

**Q: Labels are hidden behind gizmos. (레이블이 기즈모에 가립니다)**  
**EN**: We draw labels in GUI overlay or set `Handles.zTest = Always` (Unity 2021.2+).  
**KR**: GUI 오버레이(2D)로 라벨을 그리거나, Unity 2021.2+에서는 `Handles.zTest = Always` 로 깊이 버퍼를 무시할 수 있습니다.

---

### Recent Additions / 최근 추가 기능

* `autoStart`: 활성화(Enable) 시 자동으로 경로 추종 시작  
* Progress-keeping `SwitchPath()` 내장  
* SpeedCurve 키 동기화 함수 (`SyncSpeedCurveKeys`)  
* Full 3D support (Vector3-based points & handles)

---

## Roadmap

* Optional Catmull‑Rom / Hermite spline support  
* Timeline / Playables integration  
* Event tracks (invoke callbacks at ratios)  
* Runtime path editing tools

---

## License

**EN**  
Default: MIT (feel free to change). Please include attribution if you redistribute modified code.

**KR**  
기본 라이선스는 MIT입니다(필요 시 변경 가능). 수정한 코드를 재배포할 경우 출처 표기를 권장합니다.

---

## Contact

**EN**  
* Author: CodeAssetShelter  
* Email: garrettales@gmail.com  

**KR**  
* 제작자: CodeAssetShelter  
* 이메일: garrettales@gmail.com  

---

**Happy pathing! / 즐거운 길뚫 하세요!** 🎮
