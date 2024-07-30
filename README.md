# langchain_study_basic
🌼 Langchain 공부 내용 기록 repository

### 📍 교재 및 사이트 
- 주교재
    - 랭체인으로 LLM 기반의 AI 서비스 개발하기
- 참고 사이트
    - [Langchain api reference ](https://api.python.langchain.com/en/latest/langchain_api_reference.html)
    - [Wikidocs Langchain 한국어 튜토리얼](https://wikidocs.net/book/14314)
    - [Langchain v0.2](https://python.langchain.com/v0.2/docs/versions/v0_2/)

### 📍 간단한 챗봇 웹사이트 
- 파일명 : **ch5_1_simple_chatbot.ipynb**
- 라이브러리 
    - langchain
    - streamlit
    - openai
- 언어 모델 : gpt-4-turbo
- streamlit 이용해 웹사이트 아주 간단하게! 제작
- 챗봇 사용 방법
    - 개인 Open AI 키를 .env 파일에 저장하기 
    - 깃허브에서 다운로드 받은 파일을 주피터 노트북에서 켜서 File > Save and Export notebook as > Executable Script로 파일 다운받기 → .py 파일이 다운받아질 것임 
    - 터미널에서 .env랑 .py 파일을 같은 경로에 두고 'streamlit run 파일명.py'로 실행하면 웹사이트로 들어가짐 
    - 단, streamlit pip 설치되어 있어야 함 (pip install streamlit)

**직접 실행한 결과 이미지**
![image](./images/simple_chatbot_ex.png)

### 📍 PDF 요약 웹사이트 
- 파일명 :**ch_5_3_PDFsummarywebsite.ipynb**
- 라이브러리
    - langchain
    - streamlit
    - PyPDF2
    - langchain_huggingface
- 언어 모델 : gpt-3.5-turbo-16k
- 벡터 데이터베이스 : FAISS
- 임베딩 모델 : HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2"
)

**직접 실행한 결과 이미지**
1. 첫 화면
![image](./images/pdf_summary_0.png)
2. pdf 파일 업로드 
![image](./images/pdf_summary_1.png)
3. 요약 결과 확인 
![image](./images/pdf_summary_final.png)

**오류났었던 부분 기록**
> 파일을 업로드하면 요약 결과 부분에 다음 사진에 있는 내용만 나오고 제대로 요약하지 못함
![image](./images/pdf_summary_problem.png)

- 해결 시도 
    1. 사진 속 내용이 PDF 파일을 그대로 줘서 요약하지 못한다는 내용 같아 코드를 다시 봤는데 분명히 PyPDF2를 이용해 pdf 파일의 텍스트를 뽑아 chunk로 분할하고 임베딩 변환을 수행했다. 
    2. **모델 변경** : 모델을 gpt-4-turbo를 썼는데 이 부분이 혹시 잘못되었나 싶어 gpt-3-turbo-16k로 했는데 제대로 동작함!!!

👉🏻 아마 모델의 자체 제공 기능과 맞지 않거나 모델과 사용한 함수나 라이브러리 간 호환성이 맞지 않았던 것 같다. gpt-4-turbo-preview로 실험한 결과, gpt-4-turbo와 같이 아예 안되지는 않았지만, 요약은 해주나 다음과 같은 문장이 나왔다. 
![image](./images/pdf_summary_problem_test.png)

### 📍 PDF 기반 RAG Chatbot
> 사용자가 업로드한 PDF를 가지고 RAG 기반 답변을 수행하는 챗봇이다. 

- 사용자의 질문이 들어오면 먼저 PDF 파일에서 내용을 찾아보고 질문과 찾은 결과를 LLM에 제공해 답변을 하는 방식이다. 
- 언어 모델 : gpt-3.5-turbo-16k
- ConversationBufferWindowMemory를 사용해 최근 k개의 대화에 대해서는 기억하고 있음 
- LangChain에 제공하는 Retrieval 기반 QA 클래스에는 **RetrievalQA**와 **ConversationalRetrievalChain**의 2가지 종류가 있음 
    - RetreivalQA : Retrieval만 사용해 답변 생성
    - ConversationalRetrievalChain : Chat History와 Retrieval을 함께 사용해 답변 생성 
    
        👉🏻 chat_history.append((query, result["result"]로 {"chat_history": chat_history}에 질문과 답변의 대화 내역을 저장함 
 
- 벡터 데이터베이스로는 FAISS를 사용함
- 아쉬운 점 : 대화형 챗봇이 아니다. 
    
    👉🏻 따라서 질문 하나를 하고, 또 하나를 다시 보내면 기존 답장이 지워지고 새로운 답장이 보여지는 형식임. 🌈 다음 예정 : 대화형 챗봇 제작 

**직접 실행한 결과 이미지**
1. 실행 시 첫 화면
![image](./images/pdf_rag_chat_0.png)
2. pdf 파일 업로드 후 Upload 버튼 눌렀을 때 화면 
![image](./images/pdf_rag_chat_1.png)
3. 질문 적은 화면
![image](./images/pdf_rag_chat_2.png)
4. 질문에 대한 답변 생성된 화면
![image](./images/pdf_rag_chat_3.png)