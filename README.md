# graph DB(neo4j) 활용 영화 추천
## graph RAG 소개
- Graph 데이터베이스를 기반으로 한 질의 응답 시스템(GraphRAG)은 전통적인 벡터 기반 RAG 시스템보다 더 정확하고 연관성 있는 답변을 제공함. 
- 자연어 질문을 Neo4j Cypher 쿼리로 변환하여 지식 그래프를 효과적으로 탐색함.

### 특장점:
- 정확한 관계 검색: 그래프 데이터베이스의 관계 중심 구조를 활용해 복잡한 연결 패턴을 찾을 수 있음.
- 컨텍스트 유지: 엔티티 간의 관계를 유지하여 더 풍부한 컨텍스트를 제공함.
구조화된 정보 검색: 단순 텍스트 검색이 아닌 구조화된 방식으로 정보를 검색함.


# 설치 라이브러리
```
pip install langchain, langchain-neo4j, langchain-openai
pip install pandas
```
# 실습 설명
## 실습 개요
| 실습   | 목적                    |
| ---- | --------------------- |
| 실습 1 | CSV 기반 영화 그래프 DB 구축   |
| 실습 2 | Cypher 기본 검색          |
| 실습 3 | Full-text Search      |
| 실습 4 | Vector Search         |
| 실습 5 | 자연어 → Cypher 변환       |
| 실습 6 | Graph + Vector RAG 결합 |

## 실습1: Neo4j 영화 Graph DB 구축
- 파일 : 1.neo4j_movie_graphdb구축_csv.ipynb
### 내용
- CSV 파일로부터 영화, 배우, 감독, 장르 데이터를 Pandas로 읽어옴

- 코드 흐름:
    1. CSV 로딩
    2. 노드/관계 스키마 정의
    3. MERGE 기반 중복 방지 적재

- 주요 노드:
    - Movie
    - Person (Actor / Director)
    - Genre

- 주요 관계:
    - ACTED_IN
    - DIRECTED
    - IN_GENRE    

- DB 저장: 
    - langchain_neo4j의 GraphDocument를 활용해 정의된 노드와 관계를 Neo4j 데이터베이스에 일괄 저장

→ 그래프를 이후 모든 검색과 RAG 단계에서 공통 데이터 레이어로 재사용하도록 설계함

## 실습2: Cypher 기반 기본 영화 검색
- 파일 : 2.neo4j_movie_basic_search.ipynb
### 내용
- Cypher 문법 기반 패턴 매칭 구조 이해

- 코드 관점 핵심:
    - MATCH (노드)-[관계]->(노드) 구조 해석
    - WHERE 조건과 관계 방향의 의미

- 예시:
    - 특정 장르 영화 조회
    - 특정 배우가 출연한 영화 검색    

→ Graph DB 관계 기반 탐색 방식 이해에 초점을 맞춤

## 실습3: Full-text Search를 활용한 검색
- 파일 : 3.neo4j_movie_full-text_search.ipynb
### 내용
- 부분 일치나 복합 키워드를 포함하는 영화 정보를 효율적으로 검색하는 방법을 실습

- 코드 흐름:
    1. 텍스트 인덱스 정의
    2. 키워드 기반 검색 쿼리 실행

- 검색 대상:
    - 영화 제목
    - 설명(overview)

→ 관계 기반 탐색과 텍스트 기반 탐색의 역할 차이를 비교

## 실습4: Vector Search 기반 영화 검색
- 파일 : 4.neo4j_movie_vector_search.ipynb
### 내용
- 임베딩 생성: 영화의 줄거리(Overview)를 OpenAI 임베딩 모델을 통해 벡터로 변환

- 벡터 속성 저장: 
    - db.create.setNodeVectorProperty를 사용하여 Neo4j 노드에 벡터 정보 저장

- 유사도 검색: 
    - 벡터 유사도 측정을 통해 의미적으로 유사한 영화를 찾아내는 기술 구현

→ Vector Search가 Graph Search를 대체하지 않고 보완함을 확인

## 실습5: Text-to-Cypher 구현
- 파일 : 5.neo4j_movie_text2cypher.ipynb
### 내용
- 자연어 질문을 Cypher 쿼리로 변환
- LangChain + OpenAI API 활용

- 코드 흐름:
    1. 사용자 자연어 입력
    2. 프롬프트 기반 쿼리 생성
    3. 생성된 Cypher 실행

- 예시 질의:
    - 액션 장르이면서 평점이 높은 영화 추천

→ LLM이 그래프 질의 생성 도구로 활용될 수 있음을 구조적으로 검증

## 실습6: Graph + Vector RAG 결합 시스템
- 파일 : 6.neo4j_movie_graphVector_RAG.ipynb
### 내용
- Graph Search와 Vector Search를 결합한 RAG 구조 구현

- 처리 흐름:
    1. 사용자 자연어 질문 입력
    2. Graph 기반 구조적 필터링
    3. Vector 기반 의미 검색
    4. 검색 결과를 컨텍스트로 LLM 응답 생성

→ 검색된 그래프 컨텍스트를 LLM(GPT)에 전달해 영화에 대한 전문적인 질문에 답변하는 최종 RAG 시스템 구현

## 실습7
- 파일 : 7.neo4j_movie_graphRAG_hybrid.ipynb

### Neo4j 기반 하이브리드 RAG
- Vector RAG + Graph RAG를 활용한 서비스 구현하기

