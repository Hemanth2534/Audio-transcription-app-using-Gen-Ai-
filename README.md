
import openai
import os

# Set your OpenAI API key
openai.api_key = "AIzaSyBH8M4ecrIQFtzI3RX4eWEVKIYkxMCKbeo"  # Replace with your actual API key

def transcribe_audio(audio_file_path):
    """Transcribes an audio file using OpenAI's Whisper API."""
    try:
        with open(audio_file_path, "rb") as audio_file:
            transcript = openai.Audio.transcribe(
                model="whisper-1",  # You can also use "whisper-1"
                file=audio_file,
                response_format="text" # can be "text", "json", "srt", "verbose_json", or "vtt"
            )
        return transcript
    except Exception as e:
        print(f"Error: {e}")
        return None

if _name_ == "_main_":
    audio_file = "audio.mp3"  # Replace with your audio file path
    transcript = transcribe_audio(audio_file)

    if transcript:
        print("Transcription:")
        print(transcript)

        # Optionally, save the transcription to a file
        with open("transcription.txt", "w") as f:
            f.write(transcript)
        print("Transcription saved to transcription.txt")
    else:
        print("Transcription failed.")
