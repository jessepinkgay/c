#ifndef USER_H
#define USER_H
#include <time.h>

typedef struct client_s{
    int studentnum;
    char password[15];
    char name[20];
    char address[30];
    char phonenum[20];
    struct client_s *next;
} client_s;

typedef struct book_s{
    int booknum;
    char bookname[30];
    char publisher[30];
    char author[20];
    long long ISBN;
    char location[30];
    char available;
    struct book_s *next;
} book_s;

typedef struct borrow_s{
    int studentnum;
    int booknum;
    time_t borrowdate;
    time_t returndate;
    struct borrow_s *next;
} borrow_s;

extern client_s *client_head;
extern book_s *book_head;
extern borrow_s *borrow_head;
extern int loop_main;


// main.c
void get_client(void);
void get_book(void);
void get_borrow(void);
void sort_clients(void); // 
void sort_books(void); // 
int libservice(void);
int membership(void);
int login(void);
void save_all_data(void);
// extern int loop_main;
extern int N;



// admin.c
void admin_func();
void register_book(void);
void delete_book(void);
void borrow_book(void);
void return_book(void);
void search_books(void);
void list_clients(void);
void save_books_to_file(void);
void save_borrows_to_file(void);
void save_all_data(void);
void format_date_korean(time_t time, char *formatted_date, size_t size);
const char* get_korean_weekday(int wday);
void print_return_date(time_t time);


// client.c
void client_func(int id);
void searchBook(void);
void searchTitle(void);
void searchPublisher(void);
void searchISBN(void);
void searchAuthor(void);
void searchAll(void);
void myBook(int id);
void edit(int id);
void withdraw(int id);
void printBook(char title[]);


typedef struct bookTitle {
	char title[30];
	struct bookTitle * next;
} bookTitle;

#endif
#include <stdio.h>
#include <time.h>
#include <string.h>
#include <stdlib.h>

client_s *client_head = NULL;
book_s *book_head = NULL;
borrow_s *borrow_head = NULL;
int loop_main = 1;
int N = 0;

void get_client() {
    FILE *cl = fopen("client", "r");
    if (cl == NULL) {
        printf("클라이언트 파일을 열 수 없습니다.\n");
        return;
    }

    while (1) {
        client_s *temp = (client_s *)malloc(sizeof(client_s));
        if (temp == NULL) {
            printf("메모리 할당 실패\n");
            fclose(cl);
            return;
        }

        // fscanf로 데이터를 읽어옵니다.
        int result = fscanf(cl, "%d | %s | %s | %[^|] | %s", 
                            &temp->studentnum,
                            temp->password,
                            temp->name,
                            temp->address,
                            temp->phonenum);
        int len = strlen(temp->address);
        while (len > 0 && temp->address[len - 1] == ' ') {
            temp->address[len - 1] = '\0';
            len--;
        }

        if (result == 5) { // 성공적으로 모든 데이터를 읽은 경우
            temp->next = NULL;

            if (client_head == NULL) {
                client_head = temp; // 첫 노드인 경우
            } else {
                client_s *cur = client_head;
                while (cur->next != NULL) {
                    cur = cur->next;
                }
                cur->next = temp; // 마지막 노드에 추가
            }
        } else { // fscanf 실패 시 메모리 해제 및 루프 종료
            free(temp);
            break;
        }
    }

    fclose(cl);
}


void get_book() {
    FILE *bk = fopen("book", "r");
    if (bk == NULL) {
        printf("도서 파일을 열 수 없습니다.\n");
        return;
    }

    while (1) {
        book_s *temp = (book_s *)malloc(sizeof(book_s));
        if (temp == NULL) {
            printf("메모리 할당 실패\n");
            fclose(bk);
            return;
        }

        // fscanf로 데이터를 읽어옵니다.
        int result = fscanf(bk, "%d | %s | %s | %s | %lld | %s | %c",
                            &temp->booknum,
                            temp->bookname,
                            temp->publisher,
                            temp->author,
                            &temp->ISBN,
                            temp->location,
                            &temp->available);

        if (result == 7) { // 성공적으로 모든 데이터를 읽은 경우
            temp->next = NULL;
	        N++;

            if (book_head == NULL) {
                book_head = temp; // 첫 노드인 경우
            } else {
                book_s *cur = book_head;
                while (cur->next != NULL) {
                    cur = cur->next;
                }
                cur->next = temp; // 마지막 노드에 추가
            }
        } else { // fscanf 실패 시 메모리 해제 및 루프 종료
            free(temp);
            break;
        }
    }

    fclose(bk);
}


void get_borrow() {
    FILE *br = fopen("borrow", "r");
    if (br == NULL) {
        printf("대여 파일을 열 수 없습니다.\n");
        return;
    }

    while (1) {
        borrow_s *temp = (borrow_s *)malloc(sizeof(borrow_s));
        if (temp == NULL) {
            printf("메모리 할당 실패\n");
            fclose(br);
            return;
        }

        // fscanf로 데이터를 읽어옵니다.
        int result = fscanf(br, "%d | %d | %ld | %ld",
                            &temp->studentnum,
                            &temp->booknum,
                            &temp->borrowdate,
                            &temp->returndate);

        if (result == 4) { // 성공적으로 모든 데이터를 읽은 경우
            temp->next = NULL;

            if (borrow_head == NULL) {
                borrow_head = temp; // 첫 노드인 경우
            } else {
                borrow_s *cur = borrow_head;
                while (cur->next != NULL) {
                    cur = cur->next;
                }
                cur->next = temp; // 마지막 노드에 추가
            }
        } else { // fscanf 실패 시 메모리 해제 및 루프 종료
            free(temp);
            break;
        }
    }

    fclose(br);
}


void sort_clients() {
    client_s *i, *j;
    int temp_studentnum;
    char temp_password[15], temp_name[20], temp_address[20], temp_phonenum[20];

    for (i = client_head; i != NULL; i = i->next) {
        for (j = i->next; j != NULL; j = j->next) {
            if (i->studentnum > j->studentnum) {
                // studentnum 교환
                temp_studentnum = i->studentnum;
                i->studentnum = j->studentnum;
                j->studentnum = temp_studentnum;

                // password 교환
                strcpy(temp_password, i->password);
                strcpy(i->password, j->password);
                strcpy(j->password, temp_password);

                // name 교환
                strcpy(temp_name, i->name);
                strcpy(i->name, j->name);
                strcpy(j->name, temp_name);

                // address 교환
                strcpy(temp_address, i->address);
                strcpy(i->address, j->address);
                strcpy(j->address, temp_address);

                // phonenum 교환
                strcpy(temp_phonenum, i->phonenum);
                strcpy(i->phonenum, j->phonenum);
                strcpy(j->phonenum, temp_phonenum);
            }
        }
    }
}


void sort_books() {
    if (book_head == NULL) return; // 리스트가 비어 있으면 정렬할 필요 없음

    book_s *i, *j;
    for (i = book_head; i != NULL; i = i->next) {
        for (j = i->next; j != NULL; j = j->next) {
            if (i->ISBN > j->ISBN) {
                // 노드 데이터 교환 대신 노드 자체를 교환
                // 전체 노드를 교환하기 위해 임시 포인터를 사용
                book_s temp = *i;
                *i = *j;
                *j = temp;

                // 연결 리스트의 next 포인터 복구
                book_s *temp_next = i->next;
                i->next = j->next;
                j->next = temp_next;
            }
        }
    }
}

int membership(){
    int studentnum;
    char password[15], name[20], address[20], phonenum[20];
    client_s *cur = client_head;
    printf("회원 가입을 시작합니다.\n");
    int new = 0;
    while (!new){
        new = 1;
        printf("학번: ");
        scanf("%d", &studentnum);
        while (cur !=NULL){
            if (cur->studentnum == studentnum){
                printf("이미 존재하는 학번입니다.\n");
                new = 0;
                break;
            }
            cur = cur->next;
        }
    } 
    
    printf("비밀번호: ");
    scanf(" %[^\n]", password);

    printf("이름: ");
    scanf(" %[^\n]", name);

    printf("주소: ");
    scanf(" %[^\n]", address);

    printf("전화번호: ");
    scanf(" %[^\n]", phonenum);
    
    client_s *new_client;
    new_client = (client_s *)malloc(sizeof(client_s));
    new_client->studentnum = studentnum;
    strcpy(new_client->password, password);
    strcpy(new_client->name, name);
    strcpy(new_client->address, address);
    strcpy(new_client->phonenum, phonenum);
    new_client->next = client_head;
    client_head = new_client;
    sort_clients();
    FILE *cl = fopen("client","w");
    cur = client_head;
    while (cur != NULL) {
        fprintf(cl, "%d | %s | %s | %s | %s\n",cur->studentnum, cur->password, cur->name, cur->address, cur->phonenum);
        cur = cur->next;
    }
    fclose(cl);
    printf("회원 가입이 완료되었습니다.\n\n\n"); // "Enter을 눌러 메뉴로 이동하세요." 부분 삭제
    while(getchar()!='\n');
    //system("clear"); TODO : clear 삭제 
    //client_func(studentnum);
    save_all_data();
    return 1;
}

int login() {
    client_s *now = client_head;
    char logname[15];
    char password[15];

    printf("로그인 시작\n");
    printf("학번: ");
    scanf("%s", logname);
    printf("비밀번호: ");
    scanf("%s", password);

    // 관리자 로그인
    if (strcmp(logname, "admin") == 0) {
        if (strcmp(password, "lib_admin7") == 0) {
            printf("관리자님, 반갑습니다.\n");
            // 관리자 기능 호출
            admin_func();
            return 0;
        } else {
            printf("관리자 비밀번호가 틀렸습니다.\n");
            return 0;
        }
    }

    // 일반 사용자 로그인
    int num = atoi(logname);
    if (num == 0) {
        printf("유효하지 않은 학번입니다.\n");
        return 0;
    }

    while (now != NULL) {
        if (num == now->studentnum && strcmp(now->password, password) == 0) {
            printf("%s님, 반갑습니다.\n", now->name);
            // 사용자 기능 호출
            client_func(num);
            return 0;
        }
        now = now->next;
    }

    printf("로그인명 혹은 비밀번호가 일치하지 않습니다.\n");
    return 0;
}


int libservice() {
START:
    int num;
    printf(">> 도서관 서비스 <<\n");
    printf("1. 회원 가입        2. 로그인        3. 프로그램 종료\n");
    printf("\n번호를 선택하세요: ");
    scanf("%d", &num);

    if (num == 1) {
        membership();
        // TODO
        goto START;
    } else if (num == 2) {
        login();
    } else {
        loop_main = 0;
        return 0;
    }
    return 1;
}


void save_all_data() {

    sort_clients();
    sort_books();

    // 클라이언트 데이터 저장
    FILE *client_file = fopen("client", "w");
    if (client_file != NULL) {
        client_s *cur_client = client_head;
        while (cur_client != NULL) {
            fprintf(client_file, "%d | %s | %s | %s | %s\n",
                    cur_client->studentnum, cur_client->password, cur_client->name,
                    cur_client->address, cur_client->phonenum);
            cur_client = cur_client->next;
        }
        fclose(client_file);
    }

    // 도서 데이터 저장
    FILE *book_file = fopen("book", "w");
    if (book_file != NULL) {
        book_s *cur_book = book_head;
        while (cur_book != NULL) {
            fprintf(book_file, "%d | %s | %s | %s | %lld | %s | %c\n",
                    cur_book->booknum, cur_book->bookname, cur_book->publisher,
                    cur_book->author, cur_book->ISBN, cur_book->location, cur_book->available);
            cur_book = cur_book->next;
        }
        fclose(book_file);
    }

    // 대여 데이터 저장
    FILE *borrow_file = fopen("borrow", "w");
    if (borrow_file != NULL) {
        borrow_s *cur_borrow = borrow_head;
        while (cur_borrow != NULL) {
            fprintf(borrow_file, "%d | %d | %ld | %ld\n",
                    cur_borrow->studentnum, cur_borrow->booknum,
                    cur_borrow->borrowdate, cur_borrow->returndate);
            cur_borrow = cur_borrow->next;
        }
        fclose(borrow_file);
    }
}


int main() {
    get_client();    
    get_book();
    get_borrow();
    sort_clients();
    save_all_data();
    sort_books();
    save_all_data();
        
    do {
        libservice();
    } while(loop_main);	

    return 0;
}

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include "user.h"

void admin_func() {
    char adminInput[10];
    while (1) {
        printf(">> 관리자 메뉴 <<\n");
        printf("1. 도서 등록        2. 도서 삭제        3. 도서 대여        4. 도서 반납\n");
        printf("5. 도서 검색        6. 회원 목록        7. 로그아웃         8. 프로그램 종료\n");
        printf("번호를 선택하세요: ");
        scanf("%s", adminInput);

        if (strcmp(adminInput, "1") == 0) {
            register_book();
            save_all_data();
        } else if (strcmp(adminInput, "2") == 0) {
            delete_book();
            save_all_data();
        } else if (strcmp(adminInput, "3") == 0) {
            borrow_book();
            save_all_data();
        } else if (strcmp(adminInput, "4") == 0) {
            return_book();
            save_all_data();
        } else if (strcmp(adminInput, "5") == 0) {
            search_books();
        } else if (strcmp(adminInput, "6") == 0) {
            list_clients();
        } else if (strcmp(adminInput, "7") == 0) {
            printf("관리자 로그아웃\n");
            return;
        } else if (strcmp(adminInput, "8") == 0) {
            printf("프로그램 종료\n");
            exit(0);
        } else {
            printf("잘못된 입력입니다. 다시 시도하세요.\n");
        }
    }
}

void register_book() {
    printf(">> 도서 등록 <<\n");

    // 새 도서 정보 입력 받기
    book_s *new_book = (book_s *)malloc(sizeof(book_s));
    if (new_book == NULL) {
        printf("메모리 할당 실패\n");
        return;
    }
    getchar();

    printf("도서명: ");
    scanf("%[^\n]", new_book->bookname);
    getchar();

    printf("출판사: ");
    scanf("%[^\n]", new_book->publisher);
    getchar();

    printf("저자: ");
    scanf("%[^\n]", new_book->author);
    getchar();

    printf("ISBN: ");
    scanf("%lld", &new_book->ISBN);
    getchar();

    printf("소장처: ");
    scanf("%[^\n]", new_book->location);

    // 자동 입력
    new_book->available = 'Y';

    // 도서 번호 자동 생성: 연결 리스트를 처음부터 끝까지 순회하며 가장 마지막 번호를 찾음
    int last_booknum = 0; // 초기 도서 번호 시작값
    book_s *cur = book_head;
    while (cur != NULL) {
	if (cur->booknum > last_booknum)
	            last_booknum = cur->booknum; // 마지막 도서 번호를 업데이트
	// 도서번호 확인용 코드
            // printf("\n%d\n", last_booknum);
	cur = cur->next;
    }
    last_booknum++; // 마지막 번호에서 +1
    new_book->booknum = last_booknum;

    printf("\n도서번호: %d, 대여 가능 여부: %c\n", new_book->booknum, new_book->available);

    char confirm;
    printf("등록하시겠습니까? (Y/N): ");
    scanf(" %c", &confirm);

    if (confirm != 'Y' && confirm != 'y') {
        printf("등록이 취소되었습니다.\n");
        free(new_book);
        return;
    }

    // 연결 리스트에 새 도서 추가
    new_book->next = NULL;
    if (book_head == NULL) {
        book_head = new_book; // 리스트가 비어 있으면 첫 노드로 설정
    } else {
        book_s *cur = book_head;
        while (cur->next != NULL) {
            cur = cur->next;
        }
        cur->next = new_book; // 리스트 끝에 새 노드 추가
    }

    printf("도서가 성공적으로 등록되었습니다.\n");
}


void delete_book() {
    int deleteNum;
    book_s *cur = book_head, *prev = NULL;

    printf("삭제할 도서 번호를 입력하세요: ");
    scanf("%d", &deleteNum);

    while (cur != NULL) {
        if (cur->booknum == deleteNum) {
            if (cur->available == 'Y') {
                if (prev == NULL) {
                    book_head = cur->next;
                } else {
                    prev->next = cur->next;
                }
                free(cur);
                printf("도서가 성공적으로 삭제되었습니다.\n");
                return;
            } else {
                printf("이 도서는 삭제할 수 없습니다.\n");
                return;
            }
        }
        prev = cur;
        cur = cur->next;
    }
    printf("해당 도서를 찾을 수 없습니다.\n");
}

void borrow_book() {
    int book_num, student_num;
    char confirm;
    book_s *book_cur = book_head;
    client_s *client_cur = client_head;
    borrow_s *new_borrow = (borrow_s *)malloc(sizeof(borrow_s));

    if (new_borrow == NULL) {
        printf("메모리 할당 실패\n");
        return;
    }

    printf("대여할 도서번호를 입력하세요: ");
    scanf("%d", &book_num);

    while (book_cur != NULL) {
        if (book_cur->booknum == book_num && book_cur->available == 'Y') {
            break;
        }
        book_cur = book_cur->next;
    }

    if (book_cur == NULL) {
        printf("대여 가능한 도서를 찾을 수 없습니다.\n");
        free(new_borrow);
        return;
    }

    printf("학번을 입력하세요: ");
    scanf("%d", &student_num);

    while (client_cur != NULL) {
        if (client_cur->studentnum == student_num) {
            printf("학생명: %s\n", client_cur->name);
            break;
        }
        client_cur = client_cur->next;
    }

    if (client_cur == NULL) {
        printf("회원 정보를 찾을 수 없습니다.\n");
        free(new_borrow);
        return;
    }

    printf("이 도서를 대여하시겠습니까? (Y/N): ");
    scanf(" %c", &confirm);

    if (confirm == 'Y' || confirm == 'y') {
        new_borrow->studentnum = student_num;
        new_borrow->booknum = book_num;
        new_borrow->borrowdate = time(NULL);

        struct tm *tm_info = localtime(&new_borrow->borrowdate);
        tm_info->tm_mday += 30;
        new_borrow->returndate = mktime(tm_info);

        new_borrow->next = borrow_head;
        borrow_head = new_borrow;

        book_cur->available = 'N';

        printf("도서가 대여되었습니다.\n");
    } else {
        free(new_borrow);
        printf("대여가 취소되었습니다.\n");
    }
}

// 한글 요일을 반환하는 함수
const char* get_korean_weekday(int wday) {
    const char* weekdays[] = {"일요일", "월요일", "화요일", "수요일", "목요일", "금요일", "토요일"};
    return weekdays[wday];
}

// 날짜를 "YYYY년 MM월 DD일 요일" 형식으로 반환하는 함수
void format_date_korean(time_t time, char *formatted_date, size_t size) {
    struct tm *tm_info = localtime(&time);

    char date_part[64];
    strftime(date_part, sizeof(date_part), "%Y년 %m월 %d일", tm_info);

    snprintf(formatted_date, size, "%s %s", date_part, get_korean_weekday(tm_info->tm_wday));
}

// 예제: 반납 예정일 출력 수정
void print_return_date(time_t borrowdate) {
   struct tm *tm_info = localtime(&borrowdate);

    // 기본적으로 30일 추가
    tm_info->tm_mday += 30;

    // 정규화
    mktime(tm_info);

    // 30일째 되는 날이 일요일인지 확인
    if (tm_info->tm_wday == 0) {  // 일요일인 경우
        tm_info->tm_mday += 1;   // 하루 추가
        mktime(tm_info);         // 정규화
    }

    printf("반납 예정일: %d년 %d월 %d일 %s\n",
           tm_info->tm_year + 1900, tm_info->tm_mon + 1, tm_info->tm_mday, get_korean_weekday(tm_info->tm_wday));
}

// 예제 함수: 반납 처리 로직에서 날짜 출력
void return_book() {
    int student_num, book_num;
    borrow_s *cur_borrow = borrow_head;
    borrow_s *prev_borrow = NULL;
    book_s *cur_book = book_head;
    char confirm;

    printf(">> 도서 반납 <<\n");

    printf("학번을 입력하세요: ");
    scanf("%d", &student_num);

    printf("\n>> 회원의 대여 목록 <<\n");
    int has_borrowed_books = 0;
    borrow_s *temp_borrow = borrow_head;

    while (temp_borrow != NULL) {
        if (temp_borrow->studentnum == student_num) {
            book_s *borrowed_book = book_head;
            while (borrowed_book != NULL) {
                if (borrowed_book->booknum == temp_borrow->booknum) {
                    char borrow_date[100], return_date[100];
                    format_date_korean(temp_borrow->borrowdate, borrow_date, sizeof(borrow_date));
                    format_date_korean(temp_borrow->returndate, return_date, sizeof(return_date));

                    printf("도서번호: %d\n도서명: %s\n대여일자: %s\n반납예정일: %s\n\n",
                           borrowed_book->booknum,
                           borrowed_book->bookname,
                           borrow_date,
                           return_date);
                    has_borrowed_books = 1;
                    break;
                }
                borrowed_book = borrowed_book->next;
            }
        }
        temp_borrow = temp_borrow->next;
    }

    if (!has_borrowed_books) {
        printf("대여 중인 도서가 없습니다.\n");
        return;
    }

    printf("반납할 도서번호를 입력하세요: ");
    scanf("%d", &book_num);

    cur_borrow = borrow_head;
    while (cur_borrow != NULL) {
        if ((cur_borrow->studentnum == student_num) && (cur_borrow->booknum == book_num)) {
            break;
        }
        prev_borrow = cur_borrow;
        cur_borrow = cur_borrow->next;
    }

    if (cur_borrow == NULL) {
        printf("해당 도서의 대여 기록을 찾을 수 없습니다.\n");
        return;
    }

    printf("도서 반납처리를 할까요? (Y/N): ");
    scanf(" %c", &confirm);

    if (confirm == 'Y' || confirm == 'y') {
        if (prev_borrow == NULL) {
            borrow_head = cur_borrow->next;
        } else {
            prev_borrow->next = cur_borrow->next;
        }

        cur_book = book_head;
        while (cur_book != NULL) {
            if (cur_book->booknum == book_num) {
                cur_book->available = 'Y';
                break;
            }
            cur_book = cur_book->next;
        }

        printf("도서가 반납되었습니다.\n");
        free(cur_borrow);
    } else {
        printf("반납이 취소되었습니다.\n");
    }
}


void search_books() {
    searchBook();
}

void list_clients() {
    int input;
    char name[20];
    int studentnum;

    printf(">> 회원 목록 <<\n");

    do {
        client_s *cur = client_head;
        printf("\n1. 이름 검색    2. 학번 검색\n");
        printf("3. 전체 검색    4. 이전 메뉴\n");
        printf("번호를 선택하세요: ");
        scanf("%d", &input);

        if (input == 1) { // 이름 검색
            printf("이름을 입력하세요: ");
            scanf("%s", name);
            int found = 0;
            while (cur != NULL) {
                if (strcmp(cur->name, name) == 0) {
                    printf("학번: %d, 이름: %s, 주소: %s, 전화번호: %s\n",
                           cur->studentnum, cur->name, cur->address, cur->phonenum);
                    found = 1;
                }
                cur = cur->next;
            }
            if (!found) {
                printf("해당 이름의 회원을 찾을 수 없습니다.\n");
            }
        } else if (input == 2) { // 학번 검색
            printf("학번을 입력하세요: ");
            scanf("%d", &studentnum);
            int found = 0;
            while (cur != NULL) {
                if (cur->studentnum == studentnum) {
                    printf("학번: %d, 이름: %s, 주소: %s, 전화번호: %s\n",
                           cur->studentnum, cur->name, cur->address, cur->phonenum);
                    found = 1;
                }
                cur = cur->next;
            }
            if (!found) {
                printf("해당 학번의 회원을 찾을 수 없습니다.\n");
            }
        } else if (input == 3) { // 전체 검색
            if (cur == NULL) {
                printf("등록된 회원이 없습니다.\n");
            }
            while (cur != NULL) {
                printf("학번: %d, 이름: %s, 주소: %s, 전화번호: %s\n",
                       cur->studentnum, cur->name, cur->address, cur->phonenum);
                cur = cur->next;
            }
        } else if (input == 4) { // 이전 메뉴
            printf("이전 메뉴로 돌아갑니다.\n");
        } else { // 잘못된 입력
            printf("잘못된 입력입니다. 다시 시도하세요.\n");
        }
    } while (input != 4);
}

#include <stdio.h>
#include <string.h>
#include <time.h>
#include <stdlib.h>
#include "user.h"

// extern char logname[15];

void client_func(int id);

void searchBook(void);

void searchTitle(void);
void searchPublisher(void);
void searchISBN(void);
void searchAuthor(void);
void searchAll(void);

void myBook(int id);
void edit(int id);
void withdraw(int id);
void logout(int id); // 함수로 구현 x
void finish(void); // 함수로 구현 x
void printBook(char title[]);

// 회원용 기본 메뉴
void client_func(int id) {
	int myId = id;
	int loop_client = 1;
	int choice;

	do {
		printf("\n\n");
		printf(">> 회원 메뉴 <<\n");
		printf("1. 도서  검색		2. 내 대여 목록\n");
		printf("3. 개인 정보 수정	4. 회원 탈퇴\n");
		printf("5. 로그아웃		6. 프로그램 종료\n");
		printf("번호를 선택하세요: ");

		scanf("%d", &choice);
		if (choice < 1 || choice > 6) {
			printf("1 ~ 6 사이의 버튼을 눌러주세요.\n");
			continue;
		}

		switch (choice) {
			case 1 :
				searchBook();
				break;
			case 2 :
				myBook(myId);
				break;
			case 3 :
				edit(myId);
				break;
			case 4 : 
				withdraw(myId);
				return ;
				break;
			case 5 :
				loop_client = 0;
				printf("\n\n");
				return ;
				break;
			case 6 :
				loop_client = 0;
				loop_main = 0;
				break;
		}
	} while (loop_client);

	return ;
}

// 회원용 책 검색
void searchBook(void) {
	int loop_searchBook = 1;
	int loop_choice = 1;

	while(loop_searchBook) {
		printf("\n\n");
		printf(">> 도서 검색 <<\n");
		printf("1. 도서명 검색		2. 출판사 검색\n");
		printf("3. ISBN 검색		4. 저자명 검색\n");
		printf("5. 전체 검색		6. 이전 메뉴\n");
		printf("번호를 선택하세요: ");

		int choice;
		scanf("%d", &choice);
		if (choice < 1 || choice > 6) {
			printf("1~6 사이의 번호를 입력해주세요.\n");
			continue;
		}
		
		switch (choice) {
			case 1 :
				searchTitle();
				break;
			case 2 :
				searchPublisher();
				break;
			case 3 :
				searchISBN();
				break;
			case 4 :
				searchAuthor();
				break;
			case 5 :
				searchAll();
				break;
			case 6 :
				loop_choice = 0;
				return ;
		}
	} 

	return ;
}

// 회원용 제목으로 검색
void searchTitle(void) {
	char input[100];
	printf("\n\n");
	printf("도서명을 입력하세요: ");
	getchar();
	scanf("%[^\n]", input);
	
	printBook(input);

	return ;
}


// 회원용 출판사로 검색
void searchPublisher(void) {
    char input[100];
    printf("\n\n");
    printf("출판사를 입력하세요: ");
    getchar();
    scanf("%[^\n]", input);

    char Title[N][30]; // 출판사 저장할 배열
    book_s *i;
    int count = 0;

    // 출판사 이름을 배열에 저장
    for (i = book_head; i != NULL; i = i->next) {
        if (strcmp(i->publisher, input) == 0) {// 출판사 이름이 입력된 문자열과 매칭될 때 
            strcpy(Title[count++], i->bookname);
	// printf("%s\n", i->bookname);
    }
}

    // 중복 제거
    for (int i1 = 0; i1 < count; i1++) {
        for (int i2 = i1 + 1; i2 < count; i2++) {
            if (strcmp(Title[i1], Title[i2]) == 0) {
                // 중복 제거: 요소를 밀어서 제거
                for (int k = i2; k < count - 1; k++) {
                    strcpy(Title[k], Title[k + 1]);
                }
                count--; // 요소 개수를 하나 줄임
                i2--;    // 루프 인덱스를 조정
            }
        }
    }

    // 중복 제거 후 결과 출력
    for (int i1 = 0; i1 < count; i1++) {
        printBook(Title[i1]);
    }

    return;
}


	
// 회원용 ISBN으로 검색
void searchISBN(void) {
	long long input;
	printf("\n\n");
	printf("ISBN을 입력하세요: ");
	scanf("%lld", &input);
	
	// extern book_s *book_head;
	book_s * i;
	book_s * j;
	for (i = book_head; i != NULL; i = i->next) {
		if (input == i->ISBN) {
			j = i;
		}
	}

	printBook(j->bookname);

	return ;
}


// 회원용 저자명으로 검색
void searchAuthor(void) {
	char input[100];
    printf("\n\n");
    printf("저자명을 입력하세요: ");
    getchar();
    scanf("%[^\n]", input);

    char Title[N][30]; // 출판사 저장할 배열
    book_s *i;
    int count = 0;

    // 출판사 이름을 배열에 저장
    for (i = book_head; i != NULL; i = i->next) {
        if (strcmp(i->author, input) == 0) {// 출판사 이름이 입력된 문자열과 매칭될 때 
            strcpy(Title[count++], i->bookname);
    }
}

    // 중복 제거
    for (int i1 = 0; i1 < count; i1++) {
        for (int i2 = i1 + 1; i2 < count; i2++) {
            if (strcmp(Title[i1], Title[i2]) == 0) {
                // 중복 제거: 요소를 밀어서 제거
                for (int k = i2; k < count - 1; k++) {
                    strcpy(Title[k], Title[k + 1]);
                }
                count--; // 요소 개수를 하나 줄임
                i2--;    // 루프 인덱스를 조정
            }
        }
    }

    // 중복 제거 후 결과 출력
    for (int i1 = 0; i1 < count; i1++) {
        printBook(Title[i1]);
    }

    return;
}


// 회원용 저자명으로 검색
/*
void searchAuthor(void) {
	char input[100];
	printf("\n\n");
	printf("저자를 입력하세요: ");
	getchar();
	scanf("%[^\n]", input);
	int find = 0;
	int x = 0;
	int y = 0;

	// extern book_s *book_head;
	book_s * i;
	for (i = book_head; i != NULL; i = i->next) {
		if (strcmp(input, i->author) == 0) {
			// y++;
			// if (i->available == 'Y')
				// x++;

			printf("도서명: %s\n", i->bookname);
			printf("출판사: %s\n", i->publisher);
			printf("저자명: %s\n", i->author);
			printf("ISBN: %lld\n", i->ISBN);
			printf("소장처: %s\n", i->location);
			printf("대여가능 여부: %c(%d/%d)\n", i->available, x, y); // (%d/%d) 필요
			printf("** Y는 대여가능, N은 대여불가를 의미\n");
			printf("** (x/y) : (대여된 총 권수 / 보유하고 있는 총 권수\n\n");

			find = 1;
		}
	}

	if (find == 0)
		printf("해당 책은 없습니다.\n");

	return ;
}
*/


// 회원용 책 전체 검색
void searchAll(void) {
    char Title[N][30];
    book_s *i;
    int count = 0;

    // 모든 책 이름을 Title 배열에 복사
    for (i = book_head; i != NULL; i = i->next)
        strcpy(Title[count++], i->bookname);

    // 중복 제거
    for (int i1 = 0; i1 < count; i1++) {
        for (int i2 = i1 + 1; i2 < count; i2++) {
            if (strcmp(Title[i1], Title[i2]) == 0) {
                // 중복된 요소를 제거하기 위해 Title[i2]를 Title[i2+1]로 덮어쓰기
                for (int k = i2; k < count - 1; k++) {
                    strcpy(Title[k], Title[k + 1]);
                }
                count--; // 총 개수를 줄임
                i2--;    // 한 단계 뒤로 가서 새로 덮어쓴 값을 확인
            }
        }
    }

    // 중복 제거 후 결과 출력
    for (int i1 = 0; i1 < count; i1++)
        printBook(Title[i1]);

    return;
}





// 회원용 책 전체 검색
/* void searchAll(void) {
	// extern book_s *book_head;
	printf("\n\n");
	book_s * i;

	for (i = book_head; i != NULL; i = i->next) {
		printf("도서명: %s\n", i->bookname);
		printf("출판사: %s\n", i->publisher);
		printf("저자명: %s\n", i->author);
		printf("ISBN: %lld\n", i->ISBN);
		printf("소장처: %s\n", i->location);
		printf("대여가능 여부: %c\n", i->available); // (%d/%d) 필요
		// printf("** Y는 대여가능, N은 대여불가를 의미\n");
		// printf("** (x/y) : (대여된 총 권수 / 보유하고 있는 총 권수\n\n");
		printf("\n\n");
	}		

	return ;
}
*/

// 회원용 내 대여 목록
void myBook(int id) {
	int myId = id;
	borrow_s *i;
	// extern borrow_s *borrow_head;
	// extern book_s *book_head;
	int find = 0;
	
	for (i = borrow_head; i != NULL; i = i->next) {
		if (myId == i->studentnum) {
			book_s *j = book_head;
			while (i->booknum != j->booknum)
				j = j->next;

			char get_book[100];
			char return_book[100];
			format_date_korean(i->borrowdate, get_book, sizeof(get_book));
printf("%s", get_book);
			format_date_korean(i->returndate, return_book, sizeof(return_book));

			printf("도서번호: %d\n", i->booknum);
			printf("도서명: %s\n", j->bookname);
			printf("대여일자: %s\n", get_book); // 구조체 수정 후 추가
			printf("반납일자:%s\n", return_book); // 구조체 수정 후 추가
			printf("\n");

			find = 1;
		}
	}

	if (find == 0) {
		printf("\n\n");
		printf("대여하신 책이 없습니다.\n");
	}

	return ;
}


// 회원용 정보 수정
void edit(int id) {
	// extern client_s *client_head;
	int choice;
	char input[100];
	int loop_edit = 1;

	client_s *i;
	for (i = client_head; i != NULL; i = i->next) {
		if (id == i->studentnum)
			break;
	}

	do {
		printf("\n\n");
		printf("수정할 항목을 선택하세요.\n");
		printf("1. 비밀번호\n");
		// printf("2. 이름\n");
		printf("2. 주소\n");
		printf("3. 전화번호\n");

		scanf("%d", &choice);
		if (choice < 1 || choice > 4) {
			printf("번호를 다시 입력해주세요. 1~4의 번호를 입력해야 됩니다.\n");
		}
		else
			loop_edit = 0;
	} while(loop_edit);

	switch (choice) {
		case 1 :
			printf("비밀번호를 입력해주세요:");
			scanf("%s", input);
			strcpy(i->password, input);
			break;
		case 2 :
			printf("주소를 입력해주세요:");
			getchar();
			scanf("%[^\n]", input);
			strcpy(i->address, input);
			break;
		case 3 :
			printf("전화번호를 입력해주세요:");
			scanf("%s", input);
			strcpy(i->phonenum, input);
			break;
	}
	save_all_data();
	return ;
}

// 회원 탈퇴 함수
void withdraw(int id) {
    int myId = id;
    client_s *current = client_head;
    client_s *previous = NULL;
    borrow_s * i;
   
	for (i = borrow_head; i != NULL; i = i->next) {
		if (myId == i->studentnum) {
			printf("대여 중인 책이 있으므로 회원탈퇴를 할 수 없습니다.\n\n\n");
			return ;
		}
	}

    // 연결 리스트 순회
    while (current != NULL) {
        if (current->studentnum == id) { // ID가 일치하는 회원 찾기
            if (previous == NULL) {
                // 첫 번째 노드를 삭제하는 경우
                client_head = current->next;
            } else {
                // 중간 또는 마지막 노드를 삭제하는 경우
                previous->next = current->next;
            }
			save_all_data();

            // 메모리 해제
            free(current);
            printf("회원 ID %d의 계정이 성공적으로 삭제되었습니다.\n\n", id);
	// 회원탈퇴 확인용 코드
	/*
	current = client_head;
	while(current != NULL) {
		printf("%s\n\n", current->name);
		current = current->next;
	}
	*/
            return;
        }

        // 다음 노드로 이동
        previous = current;
        current = current->next;
    }

    // ID를 찾지 못한 경우
    printf("회원 ID %d를 찾을 수 없습니다.\n", id);
}


// 책 출력 함수
void printBook(char title[]) {
	int find = 0;
	int x = 0;
	int y = 0;
	char avail;

	// extern book_s *book_head;
	book_s * i;
	book_s *p = NULL;
	for (i = book_head; i != NULL; i = i->next) {
		if (strcmp(title, i->bookname) == 0) {
			y++;
			if (i->available == 'N')
				x++;

			p = i;
			find = 1;		
		}
	}

	if (x == y)
		avail = 'N';
	else
		avail = 'Y';

	if (find == 1) {
		printf("도서명: %s\n", p->bookname);
		printf("출판사: %s\n", p->publisher);
		printf("저자명: %s\n", p->author);
		printf("ISBN: %lld\n", p->ISBN);
		printf("소장처: %s\n", p->location);
				printf("대여가능 여부: %c(%d/%d)\n", avail, x, y); // (%d/%d) 필요
		printf("** Y는 대여가능, N은 대여불가를 의미\n");
		printf("** (x/y) : (대여된 총 권수 / 보유하고 있는 총 권수\n\n");
	}
	else
		printf("해당 책은 없습니다.\n");

	return ;
}
