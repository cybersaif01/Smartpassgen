import sys
import itertools
from PyQt5.QtWidgets import (QApplication, QWidget, QLabel, QLineEdit, QPushButton,
                             QTextEdit, QVBoxLayout, QHBoxLayout, QMessageBox, QFileDialog)
from PyQt5.QtGui import QIcon

# Generate password variants
def generate_variants(words):
    variants = set()

    suffixes = ['', '123', '!', '@', '2024']

    for word in words:
        base_variants = [word.lower(), word.capitalize(), word.upper()]

        for variant in base_variants:
            for suffix in suffixes:
                variants.add(variant + suffix)

    return list(variants)

# Combine words
def combine_words(variants, min_length=6, max_length=16):
    final_words = set()

    for i in range(1, 4):
        for combo in itertools.permutations(variants, i):
            combined = ''.join(combo)
            if min_length <= len(combined) <= max_length:
                final_words.add(combined)

    return final_words

# Main Application
class SmartPassGenApp(QWidget):
    def __init__(self):
        super().__init__()
        self.init_ui()

    def init_ui(self):
        self.setWindowTitle('SmartPassGen - Custom Wordlist Generator')
        self.setWindowIcon(QIcon('lock.png'))
        self.resize(600, 500)

        # Inputs
        self.first_name = QLineEdit()
        self.first_name.setPlaceholderText('First Name')

        self.last_name = QLineEdit()
        self.last_name.setPlaceholderText('Last Name')

        self.birth_year = QLineEdit()
        self.birth_year.setPlaceholderText('Birth Year')

        self.favorite_color = QLineEdit()
        self.favorite_color.setPlaceholderText('Favorite Color')

        self.hobby = QLineEdit()
        self.hobby.setPlaceholderText('Hobby or Interest')

        # Generate Button
        self.generate_button = QPushButton('Generate Wordlist')
        self.generate_button.clicked.connect(self.generate_wordlist)

        # Save Button
        self.save_button = QPushButton('Save Wordlist to File')
        self.save_button.clicked.connect(self.save_wordlist)
        self.save_button.setEnabled(False)

        # Output
        self.output_area = QTextEdit()
        self.output_area.setReadOnly(True)

        # Layouts
        input_layout = QVBoxLayout()
        input_layout.addWidget(self.first_name)
        input_layout.addWidget(self.last_name)
        input_layout.addWidget(self.birth_year)
        input_layout.addWidget(self.favorite_color)
        input_layout.addWidget(self.hobby)

        button_layout = QHBoxLayout()
        button_layout.addWidget(self.generate_button)
        button_layout.addWidget(self.save_button)

        main_layout = QVBoxLayout()
        main_layout.addLayout(input_layout)
        main_layout.addLayout(button_layout)
        main_layout.addWidget(self.output_area)

        self.setLayout(main_layout)

    def generate_wordlist(self):
        inputs = [
            self.first_name.text().strip(),
            self.last_name.text().strip(),
            self.birth_year.text().strip(),
            self.favorite_color.text().strip(),
            self.hobby.text().strip()
        ]

        if not any(inputs):
            QMessageBox.warning(self, 'Error', 'Please fill at least one field.')
            return

        variants = generate_variants(inputs)
        self.wordlist = combine_words(variants)

        self.output_area.setText('\n'.join(sorted(self.wordlist)))
        self.save_button.setEnabled(True)

        QMessageBox.information(self, 'Success', f'Wordlist generated with {len(self.wordlist)} entries.')

    def save_wordlist(self):
        if not hasattr(self, 'wordlist') or not self.wordlist:
            QMessageBox.warning(self, 'Error', 'No wordlist to save. Please generate first.')
            return

        file_path, _ = QFileDialog.getSaveFileName(self, 'Save Wordlist', 'smart_wordlist.txt', 'Text Files (*.txt)')

        if file_path:
            try:
                with open(file_path, 'w', encoding='utf-8') as f:
                    for word in sorted(self.wordlist):
                        f.write(word + '\n')
                QMessageBox.information(self, 'Saved', f'Wordlist saved to {file_path}')
            except Exception as e:
                QMessageBox.critical(self, 'Error', f'Failed to save file: {str(e)}')

if __name__ == '__main__':
    app = QApplication(sys.argv)

    dark_stylesheet = """
    QWidget {
        background-color: #121212;
        color: #FFFFFF;
        font-family: Arial;
        font-size: 14px;
    }

    QLineEdit, QTextEdit {
        background-color: #1E1E1E;
        color: #FFFFFF;
        border: 1px solid #3A3A3A;
        border-radius: 5px;
        padding: 6px;
    }

    QPushButton {
        background-color: #3B82F6;
        color: white;
        border-radius: 6px;
        padding: 8px 12px;
    }

    QPushButton:hover {
        background-color: #2563EB;
    }
    """

    app.setStyleSheet(dark_stylesheet)

    window = SmartPassGenApp()
    window.show()

    sys.exit(app.exec_())
