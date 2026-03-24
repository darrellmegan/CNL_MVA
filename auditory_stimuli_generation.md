## 🎧 Auditory Stimulus Construction

All auditory stimuli were generated programmatically in Python to ensure precise control over frequency content, temporal structure, and acoustic onset/offset characteristics. This approach allows for reproducible stimulus generation across sites and minimizes variability introduced by manual audio processing.


---

### 🔊 Pure Tone Generation

Pure tones were generated as sinusoidal waveforms at fixed frequencies (700 Hz and 900 Hz; **250 ms**), selected to provide clearly distinguishable auditory inputs while remaining within a comfortable hearing range for pediatric populations.


#### Signal Construction

Each tone was constructed as a sine wave, using a sampling rate of 44,100 Hz to ensure accurate temporal resolution and compatibility with standard audio hardware.


#### Offset Envelope (Ramp)

To avoid abrupt stimulus onset and offset (which can artificially inflate early auditory evoked responses) all tones were shaped using a linear ramp.

- Ramp duration: **30 ms**
- Applied to both onset and offset
- Linearly increases from 0 → 1 (fade-in) and decreases from 1 → 0 (fade-out)

---

### 🗣 Consonant–Vowel (CV) Stimulus Generation

Consonant–vowel (CV) stimuli were derived from recordings ("bat" and "cat" words) obtained from a Speech-in-Noise paradigm. This approach preserves ecologically valid acoustic structure while allowing controlled manipulation of stimulus timing and phonetic content.



#### Burst Detection and Alignment

To isolate the consonant–vowel portion of each word, the stop consonant burst (e.g., /t/ in *cat* or *bat*) was automatically detected and used as a temporal anchor for trimming.

Detection steps:
- A **bandpass filter (2–8 kHz)** was applied to isolate high-frequency energy associated with stop bursts
- The analytic signal was computed using the **Hilbert transform** to estimate the amplitude envelope
- The envelope was smoothed using a median filter to reduce noise
- Peaks in the smoothed envelope were identified, and the **final prominent peak** was assumed to correspond to the consonant burst


#### Temporal Cropping

Stimuli were truncated **prior to the detected burst**, retaining only the initial consonant–vowel segment (e.g., *ba*, *ca*).

- A buffer (~175 ms) was applied before the burst to ensure sufficient cropping
- The signal was cropped from onset up to this buffered cutoff point

This procedure standardizes stimulus duration while preserving relevant phonetic information.


#### Offset Envelope (Fade-Out)

To avoid abrupt truncation artifacts and preserve naturalistic sound termination, a smooth exponential fade-out was applied to the end of each cropped waveform:

- Fade duration: ~40 ms  
- Envelope: exponential decay  

---

### ⏱ Onset Alignment (CV and Word Stimuli)

To ensure consistent temporal alignment across all speech stimuli, consonant–vowel (CV) and word recordings were aligned based on acoustic onset. This step standardizes the timing of auditory input across trials and participants, which is critical for accurate estimation of early auditory responses (e.g., P1, N1).


#### Detection of Speech Onset 

Acoustic onset was estimated using the amplitude envelope of each waveform, which identifies candidate points of rapid energy increase corresponding to speech onset.

#### Temporal Alignment

Stimuli were aligned by cropping the waveform relative to the detected onset:

- A **30 ms buffer** was included prior to the detected onset to preserve natural onset dynamics  
- The signal was truncated from this buffered onset point to the end of the recording  
- A **10 ms linear fade-in** was applied to the beginning of each aligned waveform to prevent abrupt signal onset after cropping

---

### 🔊 Loudness Normalization (LUFS)

To ensure consistent perceived loudness across all auditory stimuli, files were normalized using LUFS (Loudness Units relative to Full Scale), a perceptually informed measure of loudness. This approach provides more reliable cross-stimulus comparability than simple peak or RMS normalization, particularly for speech-based stimuli with varying temporal and spectral properties.


#### Loudness Estimation

Loudness was computed using the ITU-R BS.1770 standard via the `pyloudnorm` library:

- Integrated loudness was estimated across the full duration of each waveform  
- A block size of 100 ms was used to ensure stable loudness estimation for short stimuli  

#### Normalization Procedure

Each stimulus was normalized to a fixed target loudness:

- Target level: **−25 LUFS**  
- Loudness was adjusted using a linear gain transformation  
- The entire waveform was scaled to match the target integrated loudness  

This ensures that all stimuli are perceptually matched in loudness, independent of their spectral content.

---

### 🔊 Sound Level Calibration and Verification

Following stimulus construction and LUFS normalization, all auditory stimuli were tested using a sound level meter to assess intensity across stimulus types.


#### Measurement Setup

Sound levels were measured using a **Brüel & Kjær 2240 Sound Level Meter** under controlled conditions:

- Measurement mode: **Leq (average sound level)**
- Distance from speakers: **115 cm**
- Position: **centered between two speakers**
- Height: **94.5 cm**
- Environment: soundproof EEG booth

These parameters were selected to approximate the participant listening position during experimental recordings.


#### Calibration Procedure

- Stimuli were played through the experimental audio setup using PsychoPy  
- Each sound was presented repeatedly (~10-20s) to obtain stable Leq measurements  

#### Measured Sound Levels

The following table summarizes measured sound pressure levels (dB SPL) for each stimulus:

| Stimulus | Measured Level (dB SPL) |
|----------|-------------------------|
| 700 Hz tone | 70.0 |
| 900 Hz tone | 62.3 |
| ba | 65.8 |
| ca | 67.2 |
| cat | 65.2 |
| cab | 63.7 |
| bat | 63.7 |
| bag | 63.7 |


#### Notes for Replication

- Absolute dB levels may vary depending on speaker system, amplifier settings, and room acoustics  
- LUFS normalization ensures **relative perceptual matching**, while this calibration step ensures **absolute playback consistency**  
- Calibration should be repeated at each site using comparable measurement geometry  


#### Rationale

While LUFS normalization standardizes perceived loudness across stimuli, measurement of stimuli using a sound level meter ensures that all stimuli are delivered at similar absolute intensity levels during the experiment. This is particularly important for auditory EEG studies, where neural responses are sensitive to stimulus intensity and variability across sites must be minimized.
