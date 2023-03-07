# Video-Cutter-for-social-media
This program makes a cut to any video every 29 seconds creating different video fragments to be able to upload them to social networks. My native language is Spanish, so you will find a lot of my language in the code, you can modify it to your liking. 
Part of the code was created with ChatGPT, just make some modifications to the audio codec to make it compatible with social networks.

library Requirements:
* tkinter
* moviepy

Source Code:

    from tkinter import messagebox
    from tkinter import filedialog
    from tkinter import *
    from moviepy.video.io.VideoFileClip import VideoFileClip

# Function to select the input video file.
    def select_input_file():
        file_path = filedialog.askopenfilename()
        input_file_entry.delete(0, END)
        input_file_entry.insert(0, file_path)

# Function to select the output directory of the fragments.
    def select_output_directory():
        directory_path = filedialog.askdirectory()
        output_directory_entry.delete(0, END)
        output_directory_entry.insert(0, directory_path)

# Function to divide the video into fragments of 29 seconds.
    def split_video():
        input_file_path = input_file_entry.get()
        output_directory_path = output_directory_entry.get()

    # Open the video that you want to split into fragments
    clip = VideoFileClip(input_file_path)

    # Get the total duration of the video.
    total_duration = round(clip.duration, 2)

    # Calculate how many 29 second snippets you can get from the full video.
    num_fragments = int(total_duration / 29)

    # Calculate the duration of the last fragment.
    last_fragment_duration = total_duration - (num_fragments * 29)

    # Create a loop to iterate through each 29 second chunk.
    for i in range(num_fragments):
        # Calcula el tiempo de inicio y fin del fragmento actual.
        start_time = i * 29
        end_time = (i + 1) * 29

        # Extract the corresponding fragment and save it in a separate video file.
        fragment = clip.subclip(start_time, end_time)
        fragment_filename = f"fragment_{i}.mp4"
        fragment_filepath = output_directory_path + "/" + fragment_filename
        fragment.write_videofile(fragment_filepath, audio_codec='aac')

  # If the last part of the video is less than 29 seconds, save that part as a separate video file.
    if last_fragment_duration > 0:
        last_fragment_start_time = num_fragments * 29
        last_fragment_end_time = total_duration
        last_fragment = clip.subclip(last_fragment_start_time, last_fragment_end_time)
        last_fragment_filename = "ultimo_fragmento.mp4"
        last_fragment_filepath = output_directory_path + "/" + last_fragment_filename
        last_fragment.write_videofile(last_fragment_filepath, audio_codec='aac')
    
    # Displays a process completion message.
    messagebox.showinfo("División de video", "El video se ha dividido en fragmentos de 29 segundos.")

# Create the application window.
    root = Tk()
    root.title("Dividir video en fragmentos de 29 segundos")

# Create the user interface elements.
    input_file_label = Label(root, text="Ruta del archivo de video de entrada:")
    input_file_label.pack()
    input_file_entry = Entry(root)
    input_file_entry.pack()
    input_file_button = Button(root, text="Seleccionar archivo de video", command=select_input_file)
    input_file_button.pack()

    output_directory_label = Label(root, text="Ruta del directorio de salida:")
    output_directory_label.pack()
    output_directory_entry = Entry(root)
    output_directory_entry.pack()
    output_directory_button = Button(root, text="Seleccionar directorio de salida", command=select_output_directory)
    output_directory_button.pack()

    split_button = Button(root, text="Dividir video", command=split_video)
    split_button.pack()

# Launch the application window.
    root.mainloop()
