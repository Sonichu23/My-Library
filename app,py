import streamlit as st
import json
import os

FILE_NAME = "books.json"

def load_books():
    if os.path.exists(FILE_NAME):
        with open(FILE_NAME, "r", encoding="utf-8") as file:
            return json.load(file)
    return []

def save_books(books):
    with open(FILE_NAME, "w", encoding="utf-8") as file:
        json.dump(books, file, ensure_ascii=False, indent=4)

st.set_page_config(page_title="Управление на книги", page_icon="📚")
st.title("📚 Управление на книги")

books = load_books()

# 1. Добавяне на книга
st.subheader("Добавяне на нова книга")
with st.form("add_book_form", clear_on_submit=True):
    col1, col2 = st.columns(2)
    with col1:
        title = st.text_input("Заглавие на книгата")
    with col2:
        author = st.text_input("Автор")
    submitted = st.form_submit_button("Добави книга")
    
    if submitted:
        if title.strip() and author.strip():
            books.append({"title": title.strip(), "author": author.strip()})
            save_books(books)
            st.success(f"Книгата '{title}' беше добавена успешно!")
            st.rerun()
        else:
            st.warning("Моля, въведете заглавие и автор.")

st.divider()

# 2. Търсене, Сортиране и Брой книги
col_search, col_sort, col_count = st.columns([2, 1, 1])

with col_search:
    search_query = st.text_input("🔍 Търсене по заглавие", placeholder="Въведете име...")

with col_sort:
    st.write("")
    st.write("")
    if st.button("Сортирай А-Я", use_container_width=True):
        books.sort(key=lambda b: b["title"].lower())
        save_books(books)
        st.rerun()

with col_count:
    st.metric(label="Общ брой книги", value=len(books))

st.divider()

# 3. Показване на всички книги и Изтриване
st.subheader("Списък с книги")

filtered_books = [
    b for b in books if search_query.strip().lower() in b["title"].lower()
]

if not filtered_books:
    st.info("Няма намерени книги.")
else:
    for idx, book in enumerate(filtered_books):
        col_info, col_del = st.columns([4, 1])
        with col_info:
            st.write(f"**{book['title']}** — {book['author']}")
        with col_del:
            if st.button("Изтрий", key=f"del_{idx}_{book['title']}"):
                books.remove(book)
                save_books(books)
                st.rerun()
