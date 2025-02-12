import streamlit as st
import whisper
from pydub import AudioSegment
import os

# Set the title of the app
st.title("Audio Transcription Tool 🎤")

# Add a description
st.write("Upload an audio file (MP3 or WAV) to transcribe it into text using OpenAI's Whisper model.")

# Upload audio file
uploaded_file = st.file_uploader("Upload an audio file", type=["mp3", "wav"])

if uploaded_file is not None:
    # Display the uploaded file details
    st.audio(uploaded_file, format="audio/wav")
    st.write("File uploaded successfully!")

    # Save the uploaded file to a temporary location
    temp_file = "temp_audio.wav"
    with open(temp_file, "wb") as f:
        f.write(uploaded_file.getbuffer())

    # Convert MP3 to WAV if necessary
    if uploaded_file.name.lower().endswith(".mp3"):
        st.write("Converting MP3 to WAV...")
        audio = AudioSegment.from_mp3(temp_file)
        temp_file = "temp_audio_converted.wav"
        audio.export(temp_file, format="wav")

    # Load the Whisper model
    st.write("Loading Whisper model...")
    model = whisper.load_model("base")  # Use "base" for faster results

    # Transcribe the audio file
    st.write("Transcribing audio...")
    result = model.transcribe(temp_file)

    # Display the transcription
    st.subheader("Transcription:")
    st.write(result["text"])

    # Save the transcription to a text file
    output_file = "transcription.txt"
    with open(output_file, "w") as f:
        f.write(result["text"])

    # Provide a download link for the transcription
    st.download_button(
        label="Download Transcription",
        data=result["text"],
        file_name=output_file,
        mime="text/plain",
    )

    # Clean up temporary files
    os.remove(temp_file)
    if uploaded_file.name.lower().endswith(".mp3"):
        os.remove("temp_audio.wav")
