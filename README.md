<u>**What is been built?**</u>

A conversational AI app with the capabilities of ChatGPT deployed on Streamlit cloud enabling users to upload and retrieve information from documents by means of chat.
 
 **Why is it been built?**

Analysts in banking look through a host of publicly available documents to answer questions regarding the client's performance.

Keyboard shortcut ctrl + F is used to search specific items in the document and eloborate on the finding. 

It can become tedious:
-when the question is hard to comprehend 
-when the information on the PDF is hard to comprehend 
-when there are multiple PDFs to be checked 

Hence, the need for a conversational AI.

**How to use the app?**
1) Download the folder and run app.py
2) Execute app.py followed by streamlit run
3) App opens up in a new screen
4)Drag and drop PDF on the Your documents section
5)Click on Process to upload the document
6)Input question on the Chat section and press enter to get answer

Image of the app:
![image](https://github.com/user-attachments/assets/91f3387b-cf15-4328-b6a4-38739db23bfa)

**What are the key features?**

Tokenizing text using Tiktoken to upload PDF upto 200MB.

**How is it been build? - Short**

Front end Streamlit
LLM, Vector Embedding Open AI
Vector DB FAISS
PDP reader PYPDF2
Tokenizing text Tiktoken

PDF reader loops through individual pages of the uploaded PDFs and appends it to one long text.
Text broken down into token-based chunks using Tiktoken.
Chunks are converted to vector embedding using open AI vector embeddings.
Vector embeddings stored in FAISS.
Vector embeddings ranked base on the user input question and the best rank is retrieved by Open AI's LLM as output.
Conversation memory initialized to ensure the user input and LLM output are stored during the course of conversation enabling the LLM answer follow-up questions in context to the initial question.

**How is it been build? - Detail**

**Part 1:**
LLMs - Open AI, Models - Open AI, Vector DB - FAISS

API details created and kept in .env and that would be ignored by Git using gitignore

Streamlit application set up. Sidebar and details added. st.write, st.page_config, st.header, st.sub_header used.

if st.button and it runs the function - takes multiple PDFs. 
Loops through each PDF and initializes PDF reader. Loops through each page and then appends it in one long text.

One long text broken down into chunks and then these chunks are assigned vector representation based on the semantic meaning it conveys. While the user provides the input, again FAISS, provides a vector rep for it. Later the LLM matches the input and output.

**Part 2:**
Embedding and LLM using Open AI and it takes place in Open AI cloud infrastructure.

From LLM module -
Character text splitter is used for chunks,
Open AI embedding is used for vector embedding,
FAISS is used for storing the vector.
FAISS will store it temporarily only, not a permanent solution like Pinecone. 

Chunks are created by giving parameters - size, overlap. Size is the length of the chunks. Overlap is the buffer to ensure meaningful chunks are created.

Text chunks are converted to vector embedding using Open AI embedding which is fast but chargeable.
Some free options like instructor takes place in system but is slow.

Vector embedding is passed to FAISS by LLM. LLM is the like the nucleus of the whole setup.

Streamlit session state when variables initialized should not be changed in the session. It is used when creating the vector embedding.

Conversation memory helps the LLM to remember the previous inputs.

**Part 3:**
Both user input and LLM output are stored in session states and added to conversation memory.

OpenAI output is based on the PDF that was uploaded.

Vector embedding stored in FAISS is ranked based on the user input and the best one is retrieved by Open AI LLM.
It’s like converting both input and output to a common scale and matching it. 

The input conversion is tedious - where in PDF is converted to long text - then chunks - then vector embedding.

Conversation memory plays a key roles in setting the context right, else the user would need to keep starting afresh. And session state of streamlit helps in obtaining it.

HTML/CSS is stored in a css file and called in the main.py to provide design to the UI. 

**Part 4:**
Token based chunking rather than character based chunking. Overlap token provided as well.

Text to token and then token to text again. Encoding to token and decoding to text is done to ensure keep tabs of the token limit to ensure it is not violated. 

End and start for the token provided insider a for loop that updates for each chunk.
Update the start after each chunk is decoded and retrieved as a text.
