# Video-Cutter-for-social-media
This program makes a cut to any video every 29 seconds creating different video fragments to be able to upload them to social networks

library Requirements:
* tkinter
* moviepy

Source Code:

from tkinter import messagebox
from tkinter import filedialog
from tkinter import *
from moviepy.video.io.VideoFileClip import VideoFileClip

# Función para seleccionar el archivo de video de entrada.
def select_input_file():
    file_path = filedialog.askopenfilename()
    input_file_entry.delete(0, END)
    input_file_entry.insert(0, file_path)

# Función para seleccionar el directorio de salida de los fragmentos.
def select_output_directory():
    directory_path = filedialog.askdirectory()
    output_directory_entry.delete(0, END)
    output_directory_entry.insert(0, directory_path)

# Función para dividir el video en fragmentos de 29 segundos.
def split_video():
    input_file_path = input_file_entry.get()
    output_directory_path = output_directory_entry.get()

    # Abre el video que se desea dividir en fragmentos.
    clip = VideoFileClip(input_file_path)

    # Obtén la duración total del video.
    total_duration = round(clip.duration, 2)

    # Calcula cuántos fragmentos de 29 segundos puedes obtener del video completo.
    num_fragments = int(total_duration / 29)

    # Calcula la duración del último fragmento.
    last_fragment_duration = total_duration - (num_fragments * 29)

    # Crea un bucle para iterar por cada fragmento de 29 segundos.
    for i in range(num_fragments):
        # Calcula el tiempo de inicio y fin del fragmento actual.
        start_time = i * 29
        end_time = (i + 1) * 29

        # Extrae el fragmento correspondiente y guárdalo en un archivo de video separado.
        fragment = clip.subclip(start_time, end_time)
        fragment_filename = f"fragment_{i}.mp4"
        fragment_filepath = output_directory_path + "/" + fragment_filename
        fragment.write_videofile(fragment_filepath, audio_codec='aac')

  # Si la última parte del video no alcanza los 29 segundos, guarda ese fragmento en un archivo de video separado.
    if last_fragment_duration > 0:
        last_fragment_start_time = num_fragments * 29
        last_fragment_end_time = total_duration
        last_fragment = clip.subclip(last_fragment_start_time, last_fragment_end_time)
        last_fragment_filename = "ultimo_fragmento.mp4"
        last_fragment_filepath = output_directory_path + "/" + last_fragment_filename
        last_fragment.write_videofile(last_fragment_filepath, audio_codec='aac')
    
    # Muestra un mensaje de finalización del proceso.
    messagebox.showinfo("División de video", "El video se ha dividido en fragmentos de 29 segundos.")

# Crear la ventana de la aplicación.
root = Tk()
root.title("Dividir video en fragmentos de 29 segundos")

# Crear los elementos de la interfaz de usuario.
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

# Iniciar la ventana de la aplicación.
root.mainloop()
