# Library-Management-System
 #include <iostream>
#include <vector>
#include <algorithm>
#include <string>

class Book {
private:
    int bookID;
    std::string title;
    std::string author;
    bool isIssued;

public:
    Book(int id, std::string t, std::string a) : bookID(id), title(t), author(a), isIssued(false) {}

    int getBookID() { return bookID; }
    std::string getTitle() { return title; }
    std::string getAuthor() { return author; }
    bool getIsIssued() { return isIssued; }

    void issueBook() { isIssued = true; }
    void returnBook() { isIssued = false; }
};

class Library {
private:
    std::vector<Book> books;

public:
    void addBook(Book book) {
        books.push_back(book);
    }

    void searchBookByID(int id) {
        for (auto &book : books) {
            if (book.getBookID() == id) {
                std::cout << "Book Found: " << book.getTitle() << " by " << book.getAuthor() << std::endl;
                return;
            }
        }
        std::cout << "Book not found." << std::endl;
    }

    void searchBookByTitle(std::string title) {
        for (auto &book : books) {
            if (book.getTitle() == title) {
                std::cout << "Book Found: ID " << book.getBookID() << ", by " << book.getAuthor() << std::endl;
                return;
            }
        }
        std::cout << "Book not found." << std::endl;
    }

    void issueBook(int id) {
        for (auto &book : books) {
            if (book.getBookID() == id && !book.getIsIssued()) {
                book.issueBook();
                std::cout << "Book issued." << std::endl;
                return;
            }
        }
        std::cout << "Book not available for issue." << std::endl;
    }

    void returnBook(int id) {
        for (auto &book : books) {
            if (book.getBookID() == id && book.getIsIssued()) {
                book.returnBook();
                std::cout << "Book returned." << std::endl;
                return;
            }
        }
        std::cout << "Book not found or not issued." << std::endl;
    }

    void listAllBooks() {
        std::sort(books.begin(), books.end(), [](Book &a, Book &b) { return a.getBookID() < b.getBookID(); });
        for (auto &book : books) {
            std::cout << "ID: " << book.getBookID() << ", Title: " << book.getTitle() << ", Author: " << book.getAuthor() << ", Status: " << (book.getIsIssued() ? "Issued" : "Available") << std::endl;
        }
    }

    void deleteBook(int id) {
        auto it = std::remove_if(books.begin(), books.end(), [id](Book &book) { return book.getBookID() == id; });
        if (it != books.end()) {
            books.erase(it, books.end());
            std::cout << "Book deleted." << std::endl;
        } else {
            std::cout << "Book not found." << std::endl;
        }
    }
};

int main() {
    Library lib;
    int choice, id;
    std::string title, author;

    while (true) {
        std::cout << "1. Add Book\n2. Search Book by ID\n3. Search Book by Title\n4. Issue Book\n5. Return Book\n6. List All Books\n7. Delete Book\n8. Exit\n";
        std::cin >> choice;

        switch (choice) {
            case 1:
                std::cout << "Enter Book ID: ";
                std::cin >> id;
                std::cout << "Enter Book Title: ";
                std::cin.ignore();
                std::getline(std::cin, title);
                std::cout << "Enter Book Author: ";
                std::getline(std::cin, author);
                lib.addBook(Book(id, title, author));
                break;
            case 2:
                std::cout << "Enter Book ID: ";
                std::cin >> id;
                lib.searchBookByID(id);
                break;
            case 3:
                std::cout << "Enter Book Title: ";
                std::cin.ignore();
                std::getline(std::cin, title);
                lib.searchBookByTitle(title);
                break;
            case 4:
                std::cout << "Enter Book ID: ";
                std::cin >> id;
                lib.issueBook(id);
                break;
            case 5:
                std::cout << "Enter Book ID: ";
                std::cin >> id;
                lib.returnBook(id);
                break;
            case 6:
                lib.listAllBooks();
                break;
            case 7:
                std::cout << "Enter Book ID: ";
                std::cin >> id;
                lib.deleteBook(id);
                break;
            case 8:
                return 0;
            default:
                std::cout << "Invalid choice. Try again." << std::endl;
        }
    }
}
