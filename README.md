import sqlite3

conn = sqlite3.connect('AnimalKingdom.db')
cursor = conn.cursor()

cursor.execute('DROP TABLE IF EXISTS Animals')

cursor.execute('''
CREATE TABLE IF NOT EXISTS Animals (
    ID INTEGER PRIMARY KEY AUTOINCREMENT,
    Назва_звіра TEXT,
    Тип_звіра TEXT
)
''')


animals = [
    ('Лев', 'Ссавець'),
    ('Крокодил', 'Плазун'),
    ('Орел', 'Птах'),
    ('Морська черепаха', 'Плазун'),
    ('Мавпа', 'Ссавець')
]
cursor.executemany('INSERT INTO Animals (Назва_звіра, Тип_звіра) VALUES (?, ?)', animals)

cursor.execute('UPDATE Animals SET Назва_звіра = ? WHERE Назва_звіра = ?', ('Сокіл', 'Орел'))

cursor.execute('SELECT * FROM Animals WHERE Тип_звіра = ?', ('Ссавець',))

mammals = cursor.fetchall()
print('Звірі типу "Ссавець":')
for animal in mammals:
    print(f"{animal[0]}) {animal[1]}, {animal[2]}")

cursor.execute('SELECT * FROM Animals')
all_animals = cursor.fetchall()
print('Всі записи про звірів:')
for animal in all_animals:
    print(f"{animal[0]}) {animal[1]}, {animal[2]}")

conn.commit()
conn.close()
