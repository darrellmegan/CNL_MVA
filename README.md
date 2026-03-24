# CNL_MVA
Multimodal auditory EEG paradigm (CNL, Einstein, S. Molholm) integrating MMN, tones, CV sounds, speech, and resting-state recordings with synchronized BioSemi EEG and EyeLink eye-tracking via Python + LSL.

## 📬 Contact

**Cognitive Neurophysiology Laboratory**  
Albert Einstein College of Medicine  

**Sophie Molholm, PhD**  
sophie.molholm@einsteinmed.edu  

## 🧪 Study Overview

Minimally verbal individuals with autism spectrum disorder (ASD) are often excluded from traditional cognitive paradigms due to task demands.  
This project uses a passive auditory EEG paradigm to assess early sensory encoding and oscillatory dynamics without requiring explicit responses.

## 🎧 Paradigm Overview

- Passive auditory listening task
- No behavioral response required
- Duration: ~1-2 hours with breaks
- Designed for pediatric and minimally verbal populations

### Stimuli

- [e.g., tones / speech / oddball]
- Duration: ___ ms
- Inter-stimulus interval: ___ ms

## 🧠 Data Acquisition

- EEG (BioSemi recommended)
- Optional: eye-tracking (EyeLink)

### Suggested EEG Settings
- Sampling rate: 512 Hz
- Reference: CMS/DRL
- Channels: 32

## 🔌 Triggers

Triggers are sent via:
- LSL (default)

| Event        | Trigger Code |
|-------------|-------------|
| Stimulus A  | 1           |
| Stimulus B  | 2           |
| Fixation    | 10          |
