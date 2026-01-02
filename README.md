# AgriVoice 🌿

## Project Overview
AgriVoice is an AI-powered plant disease detection system with multilingual voice support, designed to help farmers identify crop diseases and get expert agricultural advice through an intelligent chatbot.

---

## How to Use This Project

### Prerequisites
Before you begin, ensure you have the following installed:
- **Python 3.8+** (recommended: Python 3.9 or 3.10)
- **MongoDB** (running locally on `localhost:27017`)
- **Git** (for cloning the repository)
- **Microphone** (for voice input features)

### Installation Steps

#### 1. Clone the Repository
```bash
git clone https://github.com/sahaniom/AgriVoice.git
cd AgriVoice
```

#### 2. Create Virtual Environment (Recommended)
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/Mac
python3 -m venv venv
source venv/bin/activate
```

#### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

#### 4. Set Up MongoDB
- Install MongoDB Community Edition from [mongodb.com](https://www.mongodb.com/try/download/community)
- Start MongoDB service:
  ```bash
  # Windows
  net start MongoDB
  
  # Linux/Mac
  sudo systemctl start mongod
  ```
- MongoDB will create the `agrivoice_auth` database automatically on first run

#### 5. Configure Environment Variables
Create a `.env` file in the root directory:
```bash
GEMINI_API_KEY=your_google_gemini_api_key_here
```

**To get your Gemini API Key:**
1. Go to [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Copy and paste it in the `.env` file

#### 6. Add the CNN Model
- Place your trained model file `final_model.keras` in the `model/` directory
- Create the model directory if it doesn't exist:
  ```bash
  mkdir model
  ```
- The model should be at: `model/final_model.keras`

### Running the Application

#### Start the Streamlit App
```bash
streamlit run app.py
```

The application will open in your default web browser at `http://localhost:8501`

### Usage Guide

#### 1. **User Registration & Login**
- First-time users: Click on "Signup" tab
- Enter your full name, 10-digit mobile number, and password
- After registration, login with your mobile number and password

#### 2. **Disease Detection**
- Upload a clear image of a plant leaf or fruit (JPG, JPEG, or PNG)
- The AI model will analyze and predict the disease
- View the prediction result with the disease name

#### 3. **Voice Chat with AI**
- Select your preferred language from the sidebar (12 languages supported)
- Click "Open Chatbot" to start the conversation
- **Text Input**: Type your agricultural questions directly
- **Voice Input**: 
  - Click "🎤 Start Recording"
  - Speak your question (max 7 seconds)
  - Click "🟥 Stop Recording"
  - The system will transcribe and respond in your selected language

#### 4. **Image-Based Chatbot Analysis**
- Option 1: Use the disease detection image by clicking "📤 Use Disease Detection Image"
- Option 2: Upload a new image for chatbot discussion
- Ask questions about the plant disease, treatment, or farming advice
- The AI will provide personalized responses with audio output

#### 5. **Logout**
- Click the "🚪 Logout" button in the sidebar to end your session

---

## 1. Dataset Description

### Dataset Overview
The model is trained on a comprehensive plant disease dataset covering **38 different disease classes** across **14 plant species**.

**Dataset Statistics:**
- 📊 **Total Classes**: 38 (including healthy plants)
- 🌱 **Plant Species**: 14 types
- 🖼️ **Image Resolution**: 224×224 pixels (RGB)
- 🎯 **Disease Categories**: Fungal, Bacterial, Viral infections

### Supported Plant Species & Diseases

#### 🍎 **Apple** (4 classes)
- Apple Scab
- Black Rot
- Cedar Apple Rust
- Healthy

#### 🫐 **Blueberry** (1 class)
- Healthy

#### 🍒 **Cherry** (2 classes)
- Powdery Mildew
- Healthy

#### 🌽 **Corn (Maize)** (4 classes)
- Cercospora Leaf Spot / Gray Leaf Spot
- Common Rust
- Northern Leaf Blight
- Healthy

#### 🍇 **Grape** (4 classes)
- Black Rot
- Esca (Black Measles)
- Leaf Blight (Isariopsis Leaf Spot)
- Healthy

#### 🍊 **Orange** (1 class)
- Huanglongbing (Citrus Greening)

#### 🍑 **Peach** (2 classes)
- Bacterial Spot
- Healthy

#### 🌶️ **Pepper (Bell)** (2 classes)
- Bacterial Spot
- Healthy

#### 🥔 **Potato** (3 classes)
- Early Blight
- Late Blight
- Healthy

#### 🍓 **Raspberry** (1 class)
- Healthy

#### 🫘 **Soybean** (1 class)
- Healthy

#### 🎃 **Squash** (1 class)
- Powdery Mildew

#### 🍓 **Strawberry** (2 classes)
- Leaf Scorch
- Healthy

#### 🍅 **Tomato** (10 classes)
- Bacterial Spot
- Early Blight
- Late Blight
- Leaf Mold
- Septoria Leaf Spot
- Spider Mites (Two-spotted Spider Mite)
- Target Spot
- Tomato Yellow Leaf Curl Virus
- Tomato Mosaic Virus
- Healthy

### Disease Types Coverage
- 🦠 **Bacterial Diseases**: Bacterial Spot (Apple, Peach, Pepper, Tomato)
- 🍄 **Fungal Diseases**: Scab, Rust, Blight, Mildew, Mold, Rot
- 🦟 **Viral Diseases**: Mosaic Virus, Leaf Curl Virus, Citrus Greening
- 🕷️ **Pest Damage**: Spider Mites

---

## 2. Overall Application Architecture

```mermaid
graph TB
    subgraph "Frontend (Streamlit App)"
        UI[Web Interface]
        UI --> AuthUI[Login / Register]
        UI --> DetectUI[Disease Detection]
        UI --> ChatUI[Chatbot]
        UI --> VoiceUI[Voice Input / Output]
    end

    subgraph "Backend Services"
        Auth[User Authentication]
        DiseaseAI[CNN Disease Model]
        Chatbot[Gemini AI Chatbot]
        Voice[Multilingual Voice Engine]
    end

    subgraph "Database"
        DB[(MongoDB)]
    end

    AuthUI --> Auth
    Auth --> DB

    DetectUI --> DiseaseAI
    DiseaseAI --> Chatbot

    ChatUI --> Chatbot

    VoiceUI --> Voice
    Voice --> Chatbot
```

---

## 3. CNN Model Architecture

```mermaid
graph TB
    subgraph "Input Layer"
        A[Input Image<br/>224x224x3 RGB]
    end
    
    subgraph "Preprocessing Pipeline"
        B[Image Resize<br/>224x224]
        C[Convert to RGB]
        D[Normalization<br/>÷255.0]
        E[Reshape<br/>1,224,224,3]
    end
    
    subgraph "CNN Architecture"
        F[Convolutional Block 1<br/>Conv2D + ReLU + MaxPool]
        G[Convolutional Block 2<br/>Conv2D + ReLU + MaxPool]
        H[Convolutional Block 3<br/>Conv2D + ReLU + MaxPool]
        I[Flatten Layer]
        J[Dense Layer 1<br/>+ Dropout 0.5]
        K[Dense Layer 2<br/>+ Dropout 0.3]
        L[Output Layer<br/>38 neurons + Softmax]
    end
    
    subgraph "Output Processing"
        M[Probability Distribution<br/>38 classes]
        N[argmax<br/>Get highest probability]
        O[Disease Prediction<br/>+ Confidence Score]
    end
    
    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    I --> J
    J --> K
    K --> L
    L --> M
    M --> N
    N --> O
```

**Model Specifications:**
- **Input Shape**: 224 × 224 × 3 (RGB images)
- **Model Type**: Sequential CNN (Keras)
- **Total Classes**: 38 plant disease categories
- **Activation Functions**: ReLU (hidden layers), Softmax (output layer)
- **Regularization**: Dropout layers (0.5 and 0.3)
- **Output**: Disease class label with confidence score

---

## 4. Voice Processing Pipeline

```mermaid
graph TB
    subgraph "Voice Input Flow"
        A[User Speaks]
        B[Microphone Recording<br/>7 seconds max]
        C[Audio Capture<br/>16kHz, mono]
        D[Save as WAV file]
        E{Language?}
        F[Google Speech Recognition<br/>11 Indian Languages]
        G[Odia STT<br/>Wav2Vec2 Model]
        H[Text Transcription]
    end
    
    subgraph "Text Processing"
        I[User Text Input]
        J[Combine with Image?]
        K[Send to Gemini AI]
        L[AI Response Generation]
    end
    
    subgraph "Voice Output Flow"
        M[AI Response Text]
        N{Language?}
        O[gTTS<br/>11 Languages]
        P[Odia TTS<br/>MMS Model]
        Q[Audio Synthesis]
        R[MP3/WAV Output]
        S[Auto-play Audio]
    end
    
    A --> B
    B --> C
    C --> D
    D --> E
    E -->|Non-Odia| F
    E -->|Odia| G
    F --> H
    G --> H
    
    H --> I
    I --> J
    J --> K
    K --> L
    
    L --> M
    M --> N
    N -->|Non-Odia| O
    N -->|Odia| P
    O --> Q
    P --> Q
    Q --> R
    R --> S
```

---

## Key Technologies Used

### Core Frameworks
- **Streamlit**: Web application framework
- **TensorFlow/Keras**: Deep learning model
- **PyMongo**: MongoDB integration
- **LangChain**: AI chatbot framework

### AI & ML Models
- **Google Gemini 2.5 Flash**: Conversational AI
- **Pre-trained CNN**: Plant disease detection
- **Wav2Vec2**: Speech-to-text for Odia
- **MMS-TTS**: Text-to-speech for Odia

### Audio Processing
- **sounddevice**: Audio recording
- **soundfile**: Audio file handling
- **librosa**: Audio analysis
- **gTTS**: Text-to-speech (11 languages)
- **SpeechRecognition**: Speech-to-text

### Security & Storage
- **bcrypt**: Password hashing
- **MongoDB**: User database
- **python-dotenv**: Environment variables

---

## Supported Languages (12)
🌐 English, Hindi, Marathi, Tamil, Telugu, Bengali, Gujarati, Odia, Punjabi, Urdu, Malayalam, Kannada

---

## Features
✅ User authentication with mobile number  
✅ Plant disease detection (38 classes)  
✅ Multilingual voice chat (12 Indian languages)  
✅ AI-powered agricultural chatbot  
✅ Image-based disease analysis  
✅ Text-to-speech in native languages  
✅ Real-time voice recording  
✅ Session-based chat history (clears on logout/refresh)  

---

**Developed by:**
* Suraj Kumar Yadav
* Om Prakash


