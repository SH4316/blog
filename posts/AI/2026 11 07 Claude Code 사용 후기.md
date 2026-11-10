---
title: Claude Code 사용기
slug: claude-code-use-with-opus-5-5-as-orchestrator
date: 2026-11-07
description: ""
tags:
  - LLM
draft: false
---
### 상황
기존에 GPT Pro 200을 사용중이었고(kakao 이벤트산 pro 200), 현재는 Claude Pro 를 같이 사용해보기로 했다.

Opus 5.5 medium ~ high (with Claude Code CLI)를 Orchestrater 로 두고 Codex 를 실행하도록 해 Subagent로 GPT 6.1 Sol (high ~ xhigh)를 사용하도록 했다.

최근에 Claude 를 가입해서 써보고 있는데, orchestration 할 때 본인(opus 5.5)을 분류기 처럼 사용하는게 인상깊었다.
script 를 만들어 task 가 끝나면 깨어나 작업을 분류하고 다시 subagent 를 불러 실행시킨다. 

### Task
Minecraft 구버전 Client 를 Rust 를 이용해 랜더링 병목구간을 없에고 개선한 re-implementation 작업을 hill-climb()  방식으로 풀어보도록 시켰다.

### Issue

처음에는 Opus가 너무 아무것도 안해서 판단 업무를 맡겼는데, Codex 와 Claude 둘 중 하나만 동작하는 구조로 동작하니까 좋진 않았다.

또한 알아서 미구현 기능을 찾거나 문제점을 찾는 것을 opus 가 하길 기대했지만 잘되지 않았다.

### Solve

해결 방법으로 subagent 동작을 좀더 명확히 나눴다. "미구현 탐색, 오류 탐색, 구현" 3가지 분류로 나눠 작업을 시켰고, 알아서 수정 범위를 확장하고 오류를 찾아 수정했다.

CPU 병목도 있었는데, 대략 20개의 codex session 이 동시에 test 를 진행하면서 cpu가 oversubscribed 되는 문제가 있었고, session 별 테스트를 단순화 하고 orchestrator 가 단일 test 로 실행하고 결과를 돌려주는 방식으로 수정했다.
이때도 opus 가 테스트 주기를 잘 못잡길래 "테스트 완료 시간을 측정 -> 해당 주기마다 구현 내용을 테스트" 하도록 명시적으로 order를 내렸다.

### 기타
Codex 만 사용할 때는 Goal 로 hill-climb 문제 구현 시켰는데, orchestration 확실히 토큰을 많이 먹는 것 같다.
Orchestrater는 Orchestration 만 하는게 맞는 것 같다.