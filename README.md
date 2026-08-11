# 2026 Git Start
로컬 컴퓨터에서 추가한 내용입니다.

# 2026-git-start
오늘의 학습 목표 : GitHub와 로컬 저장소의 Merge 충돌 해결 실습

GitHub 웹에서 추가한 내용입니다.


오늘의 학습 목표: 작업자 B의 Merge 충돌 실습

오늘의 학습 목표: 작업자 A의 Git 협업 실습

# Git 협업 및 Merge 충돌 해결 실습

## 전체 흐름

```mermaid
sequenceDiagram
    autonumber

    actor A as 👤 작업자 A
    participant LocalA as 💻 A 로컬 저장소
    participant GitHub as ☁️ GitHub 원격 저장소
    participant LocalB as 💻 B 로컬 저장소
    actor B as 👤 작업자 B

    Note over A,GitHub: ① 작업자 A의 Git 협업 시작

    A->>LocalA: 파일 수정
    Note right of LocalA: 로컬 컴퓨터에서<br/>내용 추가

    LocalA->>LocalA: git add / commit
    LocalA->>GitHub: git push
    GitHub-->>LocalA: 원격 저장소 반영 완료

    Note over GitHub: ② GitHub에서도 같은 파일 수정

    GitHub->>GitHub: GitHub Web에서 내용 추가
    Note right of GitHub: 로컬과 원격에<br/>서로 다른 변경 발생

    Note over B,GitHub: ③ 작업자 B의 협업

    B->>LocalB: 저장소 clone 또는 pull
    GitHub-->>LocalB: 최신 내용 전달

    B->>LocalB: 동일 파일 수정
    LocalB->>LocalB: git add / commit
    LocalB->>GitHub: git push
    GitHub-->>LocalB: 변경사항 반영

    Note over LocalA,GitHub: ⚠️ A의 로컬과 GitHub의 내용이 달라짐

    A->>LocalA: 추가 작업 및 commit
    LocalA->>GitHub: git push

    GitHub--xLocalA: Push 거부<br/>원격 저장소에 새로운 변경 존재

    Note over LocalA,GitHub: ④ Merge 충돌 해결

    LocalA->>GitHub: git pull
    GitHub-->>LocalA: 원격 변경사항 가져오기

    LocalA->>LocalA: ⚠️ Merge Conflict 발생

    rect rgb(255, 235, 235)
        Note over A,LocalA: 충돌 구간 확인
        A->>LocalA: A/B/GitHub 변경 내용 비교
        A->>LocalA: 필요한 내용 선택 및 통합
        A->>LocalA: 충돌 표시 제거
    end

    LocalA->>LocalA: git add
    LocalA->>LocalA: git commit

    Note over LocalA,GitHub: ⑤ 충돌 해결 결과 공유

    LocalA->>GitHub: git push
    GitHub-->>LocalA: ✅ Merge 결과 반영 완료

    GitHub-->>LocalB: 이후 pull 시<br/>통합된 최신 버전 전달

    Note over A,B: 🎉 A와 B가 동일한 최신 버전을 공유
```

## 핵심 과정 요약

**로컬 수정 → Commit → Push → 다른 작업자의 수정 → 원격 변경 발생 → Pull → Merge Conflict → 충돌 해결 → Commit → Push**

```text
┌─────────────┐
│  작업자 수정 │
└──────┬──────┘
       ↓
┌─────────────┐
│ add / commit │
└──────┬──────┘
       ↓
┌─────────────┐
│    push      │
└──────┬──────┘
       ↓
┌──────────────────┐
│ 다른 위치에서 수정 │
│ GitHub / 작업자 B │
└────────┬─────────┘
         ↓
┌─────────────┐
│    pull      │
└──────┬──────┘
       ↓
    ⚠️ 충돌
       ↓
┌─────────────┐
│ 충돌 내용 확인 │
│   및 수정     │
└──────┬──────┘
       ↓
┌─────────────┐
│ add / commit │
└──────┬──────┘
       ↓
┌─────────────┐
│    push      │
└──────┬──────┘
       ↓
   ✅ 협업 완료
```

> **실습의 핵심**
>
> Git 협업 중 여러 작업자가 같은 파일을 수정하면 로컬 저장소와 원격 저장소의 변경 내용이 달라질 수 있다. 이때 원격 변경사항을 `pull`하고, 발생한 Merge Conflict를 직접 해결한 뒤 다시 `commit`과 `push`하여 하나의 최종 버전으로 통합한다.
