import tkinter as tk
from tkinter import filedialog
import os
import re

def extract_linked_notes(note_content):
    """Extrae notas enlazadas del contenido de la nota dada."""
    linked_notes = re.findall(r"\[\[(.*?)\]\]", note_content)
    return linked_notes

def find_note_path(note_name, root_directory):
    """Busca recursivamente la ruta de un archivo en un directorio y sus subdirectorios."""
    for dirpath, dirnames, filenames in os.walk(root_directory):
        for filename in filenames:
            if filename == note_name + '.md':
                return os.path.join(dirpath, filename)
    return None

def consolidate_notes(base_note_path, notes_directory):
    """
    Lee la nota base, encuentra todas las notas enlazadas y consolida el contenido de las notas enlazadas en una nueva nota.
    """
    # Verificar si la nota base existe
    if not os.path.exists(base_note_path):
        return "Nota base no encontrada."

    # Leer la nota base
    with open(base_note_path, 'r', encoding='utf-8') as file:
        base_note_content = file.read()

    # Extraer notas enlazadas
    linked_notes = extract_linked_notes(base_note_content)

    # Consolidar contenidos
    consolidated_content = base_note_content + "\n\n---\n\n"
    for note in linked_notes:
        note_path = find_note_path(note, notes_directory)
        if note_path and os.path.exists(note_path):
            with open(note_path, 'r', encoding='utf-8') as file:
                note_content = file.read()
            consolidated_content += f"## {note}\n\n{note_content}\n\n---\n\n"

    # Guardar la nota consolidada
    consolidated_note_path = os.path.join(notes_directory, 'Consolidated_Note.md')
    with open(consolidated_note_path, 'w', encoding='utf-8') as file:
        file.write(consolidated_content)

    return f"Nota consolidada creada en {consolidated_note_path}"

def select_notes_directory():
    directory = filedialog.askdirectory()
    entry_notes_dir.delete(0, tk.END)
    entry_notes_dir.insert(0, directory)

def select_base_note():
    file_path = filedialog.askopenfilename(filetypes=[("Markdown files", "*.md")])
    entry_base_note.delete(0, tk.END)
    entry_base_note.insert(0, file_path)

def run_script():
    base_note_path = entry_base_note.get()
    notes_directory = entry_notes_dir.get()
    result = consolidate_notes(base_note_path, notes_directory)
    label_result.config(text=result)

# Crear la ventana de la GUI
root = tk.Tk()
root.title("Consolidador de Notas de Obsidian")

# Selector de Directorio del Vault
tk.Label(root, text="Ruta del Vault de Obsidian:").pack()
entry_notes_dir = tk.Entry(root, width=50)
entry_notes_dir.pack()
tk.Button(root, text="Seleccionar Directorio", command=select_notes_directory).pack()

# Selector de la Nota Base
tk.Label(root, text="Ruta de la Nota Base:").pack()
entry_base_note = tk.Entry(root, width=50)
entry_base_note.pack()
tk.Button(root, text="Seleccionar Archivo", command=select_base_note).pack()

# Botón para ejecutar el script
tk.Button(root, text="Consolidar Notas", command=run_script).pack()

# Etiqueta para mostrar resultados
label_result = tk.Label(root, text="")
label_result.pack()

# Ejecutar la GUI
root.mainloop()
