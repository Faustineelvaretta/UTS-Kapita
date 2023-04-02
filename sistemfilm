import pandas as pd
import numpy as np
from sklearn.metrics.pairwise import cosine_similarity

# Membaca data buku
books = pd.read_csv("books.csv")

# Membaca data rating
ratings = pd.read_csv("ratings.csv")

# Menggabungkan data buku dan rating
data = pd.merge(books, ratings, on='bookId')

# Membuat pivot table untuk menampilkan rating tiap buku
pivot_table = data.pivot_table(index='userId', columns='title', values='rating').fillna(0)

# Mengubah pivot table menjadi array numpy
matrix = pivot_table.to_numpy()

# Menghitung cosine similarity
cosine_sim = cosine_similarity(matrix)

# Membuat dictionary untuk menyimpan index setiap buku
indices = pd.Series(books.index, index=books['title'])

# Fungsi rekomendasi buku
def recommend_books(title, cosine_sim=cosine_sim, books=books):
    # Mencari index dari buku yang dipilih
    idx = indices[title]

    # Mencari similarity score dari buku yang dipilih
    sim_scores = list(enumerate(cosine_sim[idx]))

    # Mengurutkan buku berdasarkan similarity score
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True)

    # Mengambil index dari 10 buku terbaik
    sim_scores = sim_scores[1:11]

    # Mengambil index buku
    book_indices = [i[0] for i in sim_scores]

    # Menampilkan 10 rekomendasi buku
    recommended_books = books[['title', 'genres']].iloc[book_indices]
    recommended_books['score'] = [i[1] for i in sim_scores]
    recommended_books = recommended_books.reset_index(drop=True)
    recommended_books.index += 1

    return recommended_books.to_string(index=True)

# Meminta pengguna memasukkan judul buku
query = input("Masukkan judul buku yang ingin direkomendasikan: ")

# Mencari semua buku yang memiliki substring yang diinginkan
matched_books = books[books['title'].str.contains(query, case=False)]

# Memeriksa apakah ada buku yang cocok
if len(matched_books) == 0:
    print("Tidak ada buku yang cocok dengan kriteria pencarian Anda.")
else:
    # Menampilkan daftar buku yang cocok
    print("Daftar buku yang cocok:")
    for title in matched_books['title']:
        print(title)

    # Meminta pengguna untuk memilih judul buku
    title = input("Pilih judul buku yang ingin direkomendasikan: ")

    # Memanggil fungsi rekomendasi dan mencetak hasilnya
    if title not in matched_books['title'].values:
        print("Judul buku tidak valid.")
    else:
        print(recommend_books(title))
