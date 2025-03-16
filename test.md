```mermaid
flowchart TD
    subgraph "SPEAR-TTS Architecture"
        Input[/"Input Text"/] --> S1
        
        subgraph "Stage 1: READ"
            S1["Text-to-Semantic Tokens\n(Transformer Encoder-Decoder)"] 
            PreTraining["Pre-training\n(Denoising on semantic tokens)"] -.-> S1
            BackTranslation["Back-translation\n(Creates synthetic parallel data)"] -.-> S1
        end
        
        S1 --> SemanticTokens["Semantic Tokens\n(High-level linguistic content)"]
        
        subgraph "Stage 2: SPEAK"
            SemanticTokens --> S2["Semantic-to-Acoustic Tokens\n(Trained on audio-only data)"]
            ExamplePrompt["Example Voice Prompt\n(Short sample of target voice)"] --> TokenExtraction["Extract semantic & acoustic tokens"]
            TokenExtraction --> S2
        end
        
        S2 --> AcousticTokens["Acoustic Tokens\n(Low-level acoustic details)"]
        AcousticTokens --> Vocoder["SoundStream Vocoder"]
        Vocoder --> Output[/"High-Fidelity Speech\n(Target Voice)"/]
    end
    
    subgraph "Training Data Requirements"
        MinimalParallel["Minimal Parallel Data\n(15 min text-audio pairs)"] -.-> S1
        AudioOnly["Abundant Audio-Only Data\n(No text required)"] -.-> S2
        AudioOnly -.-> PreTraining
        AudioOnly -.-> BackTranslation
    end
    
    subgraph "Key Innovations"
        style KeyInnovations fill:#f9f9f9,stroke:#333,stroke-width:1px
        DiscreteTokens["Discrete Speech Tokens\n(Semantic & Acoustic)"]
        TwoStageArchitecture["Two-Stage Architecture\n(Read & Speak)"]
        ExamplePrompting["Example Prompting\n(Speaker Identity Control)"]
        SupervisionReduction["Reduced Supervision\n(Pre-training & Back-translation)"]
    end
    
    style S1 fill:#d4f1f9,stroke:#333,stroke-width:2px
    style S2 fill:#d4f1f9,stroke:#333,stroke-width:2px
    style SemanticTokens fill:#ffe6cc,stroke:#333,stroke-width:1px
    style AcousticTokens fill:#ffe6cc,stroke:#333,stroke-width:1px
    style ExamplePrompt fill:#e6ffcc,stroke:#333,stroke-width:1px
    style MinimalParallel fill:#ffd6cc,stroke:#333,stroke-width:1px
    style AudioOnly fill:#ffd6cc,stroke:#333,stroke-width:1px
