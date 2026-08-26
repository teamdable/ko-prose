# ko-prose

LLM 에 개발 문서를 한국어로 쓰게 하면 이런 문장이 나온다.

> 살아 있는 사례를 보겠습니다
> validator 배선을 완료했습니다
> 유의미한 개선이 있었습니다

각각 live examples, wire up, significant 를 한국어 단어로 1:1 바꾼 것이다. 한국 사람은 그렇게 글을 쓰지 않는다.

ko-prose 는 이런 문장을 걷어내는 Claude Code 스킬이다. 규칙 11개는 실제 리뷰에서 거부당한 문장과 교정본에서 뽑았다.

> **English summary** — `ko-prose` is a Claude Code skill that strips the "AI voice" from Korean prose written by an LLM: calques from English, self-congratulatory framing, forced rule-of-three lists, and padding. It is aimed at developer writing, and every rule comes from a sentence that was rejected in review together with the correction that replaced it. Korean-only, because the rules are about Korean sentences.

## 무엇을 고치나

| 전 | 후 |
|---|---|
| 살아 있는 사례 (live examples) | 실사용 사례 |
| validator 배선 완료 (wire up) | validator 연결 완료 |
| 영리한 우회법 | 우회법 |
| 유의미한 개선이 있었다 | p50 응답이 120ms에서 40ms로 줄었다 |
| 좋은 질문이에요! 정확히 짚으셨습니다 | (삭제하고 본론부터) |
| 크기·속성이 다른 지면 | 크기와 속성이 다른 지면 |
| 금번 작업은 상기 사유로 지연 | 이번 작업은 위 이유로 늦어짐 |

§1부터 §9 가 문장 안의 문체, §10 이 불필요한 부연, §11 이 문어체 한자어다.

## 불필요한 부연 (§10)

개발 문서에서는 영어 번역투보다 이쪽이 더 문제다. 앞에서 이미 댄 근거를 불릿에 다시 붙이고, 측정하지 않은 수치를 시나리오로 만들고, 코드를 읽으면 나오는 조건을 굳이 부연 설명한다. 문체가 아니라 양의 문제라 §1부터 §9 를 다 통과해도 남는다.

판단 기준은 한 줄이다. **이 문장이 없으면 읽는 사람이 다른 결정을 하나?** 아니면 삭제.

## 스킬이 건드리지 않는 것

AI 티를 지우다 보면 내용까지 같이 지워진다. 그래서 고치면 안 되는 것도 규칙에 적어 뒀다. 직접 재본 숫자, 다른 방법을 써보고 안 쓴 이유, 안 되는 건 안 된다고 잘라 말한 문장이다.

마지막이 특히 그렇다. "이건 원천적으로 막을 수 없습니다" 를 완곡하게 다듬으면 "그럼 언젠가는 되나요" 라는 문의가 다시 돌아온다.

## 설치

```bash
git clone https://github.com/teamdable/ko-prose.git
ln -s "$PWD/ko-prose" ~/.claude/skills/ko-prose
```

심볼릭 링크로 걸면 `git pull` 할 때 스킬도 같이 최신이 된다. 다음 세션부터 로드된다.

문서 점검 항목만 매 세션 읽히게 하려면 홈에 링크하고 `~/.claude/CLAUDE.md` 에서 `@` 로 부른다.

```bash
ln -s "$PWD/ko-prose/doc-checklist.md" ~/.claude/ko-doc-checklist.md
echo '@ko-doc-checklist.md' >> ~/.claude/CLAUDE.md
```

## 파일 구성

| 파일 | 다루는 것 | 언제 읽히나 |
|---|---|---|
| `SKILL.md` | 문장 안의 문체 | 한국어 산문을 쓰거나 다듬거나 번역할 때 |
| `doc-style.md` | 문서 구성. 무엇을 먼저 보여주고 무엇을 빼고 어떤 어휘로 잇는가 | 문서, 리포트, 스펙처럼 여러 문단짜리 글을 통째로 쓸 때 |
| `doc-checklist.md` | 출력 직전 점검 항목 | always-load 로 걸어두는 용도 |

`SKILL.md` 앞의 frontmatter 는 Claude Code 의 스킬 로딩용이다. 다른 LLM 도구에서는 본문만 시스템 프롬프트나 룰 파일에 붙여 써도 동작한다.

## 출처와 범위

공개판은 사내 식별자, 티켓 번호, 비공개 링크를 일반 예시로 바꿨다. 규칙 문장과 전/후 대조는 원본 그대로다.

영어 산문에는 `stop-slop` 이나 `humanizer` 를 쓴다. ko-prose 는 그 둘이 다루지 않는 한국어 고유 패턴을 다룬다.

## 라이선스

MIT. [LICENSE](LICENSE) 참고.
