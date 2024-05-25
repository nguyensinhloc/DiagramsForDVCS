# Các mô hình của DVCS
## Mô hình phân cấp chức năng
```
graph LR
    A[Hệ thống quản lý phiên bản phân tán] --> B[Quản lý kho lưu trữ]
    A --> C[Quản lý phiên bản]
    A --> D[Quản lý nhánh]
    A --> E[Quản lý hợp nhất]
    A --> F[Quản lý truy cập]

    B --> B1[Tạo kho lưu trữ]
    B --> B2[Sao chép kho lưu trữ]
    B --> B3[Xóa kho lưu trữ]

    C --> C1[Commit thay đổi]
    C --> C2[Revert thay đổi]
    C --> C3[Xem lịch sử phiên bản]

    D --> D1[Tạo nhánh]
    D --> D2[Xóa nhánh]
    D --> D3[Chuyển nhánh]

    E --> E1[Hợp nhất nhánh]
    E --> E2[Giải quyết xung đột]

    F --> F1[Quản lý người dùng]
    F --> F2[Quản lý quyền truy cập]
```
```mermaid
graph LR
    A[Hệ thống quản lý phiên bản phân tán] --> B[Quản lý kho lưu trữ]
    A --> C[Quản lý phiên bản]
    A --> D[Quản lý nhánh]
    A --> E[Quản lý hợp nhất]
    A --> F[Quản lý truy cập]

    B --> B1[Tạo kho lưu trữ]
    B --> B2[Sao chép kho lưu trữ]
    B --> B3[Xóa kho lưu trữ]

    C --> C1[Commit thay đổi]
    C --> C2[Revert thay đổi]
    C --> C3[Xem lịch sử phiên bản]

    D --> D1[Tạo nhánh]
    D --> D2[Xóa nhánh]
    D --> D3[Chuyển nhánh]

    E --> E1[Hợp nhất nhánh]
    E --> E2[Giải quyết xung đột]

    F --> F1[Quản lý người dùng]
    F --> F2[Quản lý quyền truy cập]
```
### Giải thích
Nút gốc A đại diện cho hệ thống quản lý phiên bản phân tán.

Các nút con như B, C, D, E, F đại diện cho các chức năng chính của hệ thống:

B: Quản lý kho lưu trữ, bao gồm các hoạt động tạo, sao chép và xóa kho lưu trữ.

C: Quản lý phiên bản, bao gồm các hoạt động commit, revert và xem lịch sử phiên bản.

D: Quản lý nhánh, bao gồm các hoạt động tạo, xóa và chuyển nhánh.

E: Quản lý hợp nhất, bao gồm hợp nhất nhánh và giải quyết xung đột.

F: Quản lý truy cập, bao gồm quản lý người dùng và quyền truy cập.

## Mô hình dòng dữ liệu
```
graph LR
    %% Các thành phần chính
    User[Người dùng] -->|Gửi yêu cầu| ProcessRequest[Xử lý yêu cầu]
    ProcessRequest -->|Truy vấn| RepoDatabase[Kho lưu trữ]
    ProcessRequest -->|Truy vấn| VersionDatabase[Kho phiên bản]
    ProcessRequest -->|Truy vấn| BranchDatabase[Kho nhánh]
    ProcessRequest -->|Truy vấn| AccessControl[Quản lý truy cập]
    RepoDatabase -->|Trả dữ liệu| ProcessRequest
    VersionDatabase -->|Trả dữ liệu| ProcessRequest
    BranchDatabase -->|Trả dữ liệu| ProcessRequest
    AccessControl -->|Trả dữ liệu| ProcessRequest
    ProcessRequest -->|Trả kết quả| User

    %% Xử lý các yêu cầu từ người dùng
    ProcessRequest -->|Gửi yêu cầu commit| CommitProcessor[Xử lý commit]
    CommitProcessor -->|Lưu thay đổi| VersionDatabase
    CommitProcessor -->|Cập nhật nhánh| BranchDatabase

    ProcessRequest -->|Gửi yêu cầu revert| RevertProcessor[Xử lý revert]
    RevertProcessor -->|Khôi phục phiên bản| VersionDatabase

    ProcessRequest -->|Gửi yêu cầu tạo nhánh| BranchProcessor[Xử lý tạo nhánh]
    BranchProcessor -->|Tạo nhánh mới| BranchDatabase

    ProcessRequest -->|Gửi yêu cầu hợp nhất| MergeProcessor[Xử lý hợp nhất]
    MergeProcessor -->|Hợp nhất thay đổi| VersionDatabase
    MergeProcessor -->|Giải quyết xung đột| ConflictResolver[Giải quyết xung đột]
    ConflictResolver -->|Cập nhật phiên bản| VersionDatabase

    %% Xử lý quản lý truy cập
    ProcessRequest -->|Kiểm tra quyền truy cập| AccessControlProcessor[Xử lý quyền truy cập]
    AccessControlProcessor -->|Xác minh người dùng| UserDatabase[Kho người dùng]
    AccessControlProcessor -->|Cập nhật quyền truy cập| AccessControl

    %% Các kho dữ liệu
    subgraph Databases [Kho dữ liệu]
        RepoDatabase
        VersionDatabase
        BranchDatabase
        UserDatabase
    end
```
```mermaid
graph LR
    %% Các thành phần chính
    User[Người dùng] -->|Gửi yêu cầu| ProcessRequest[Xử lý yêu cầu]
    ProcessRequest -->|Truy vấn| RepoDatabase[Kho lưu trữ]
    ProcessRequest -->|Truy vấn| VersionDatabase[Kho phiên bản]
    ProcessRequest -->|Truy vấn| BranchDatabase[Kho nhánh]
    ProcessRequest -->|Truy vấn| AccessControl[Quản lý truy cập]
    RepoDatabase -->|Trả dữ liệu| ProcessRequest
    VersionDatabase -->|Trả dữ liệu| ProcessRequest
    BranchDatabase -->|Trả dữ liệu| ProcessRequest
    AccessControl -->|Trả dữ liệu| ProcessRequest
    ProcessRequest -->|Trả kết quả| User

    %% Xử lý các yêu cầu từ người dùng
    ProcessRequest -->|Gửi yêu cầu commit| CommitProcessor[Xử lý commit]
    CommitProcessor -->|Lưu thay đổi| VersionDatabase
    CommitProcessor -->|Cập nhật nhánh| BranchDatabase

    ProcessRequest -->|Gửi yêu cầu revert| RevertProcessor[Xử lý revert]
    RevertProcessor -->|Khôi phục phiên bản| VersionDatabase

    ProcessRequest -->|Gửi yêu cầu tạo nhánh| BranchProcessor[Xử lý tạo nhánh]
    BranchProcessor -->|Tạo nhánh mới| BranchDatabase

    ProcessRequest -->|Gửi yêu cầu hợp nhất| MergeProcessor[Xử lý hợp nhất]
    MergeProcessor -->|Hợp nhất thay đổi| VersionDatabase
    MergeProcessor -->|Giải quyết xung đột| ConflictResolver[Giải quyết xung đột]
    ConflictResolver -->|Cập nhật phiên bản| VersionDatabase

    %% Xử lý quản lý truy cập
    ProcessRequest -->|Kiểm tra quyền truy cập| AccessControlProcessor[Xử lý quyền truy cập]
    AccessControlProcessor -->|Xác minh người dùng| UserDatabase[Kho người dùng]
    AccessControlProcessor -->|Cập nhật quyền truy cập| AccessControl

    %% Các kho dữ liệu
    subgraph Databases [Kho dữ liệu]
        RepoDatabase
        VersionDatabase
        BranchDatabase
        UserDatabase
    end
```
### Giải thích
User: Người dùng gửi yêu cầu đến hệ thống.

ProcessRequest: Xử lý các yêu cầu từ người dùng và chuyển đến các xử lý cụ thể.

RepoDatabase, VersionDatabase, BranchDatabase, UserDatabase: Các kho dữ liệu của hệ thống.

CommitProcessor, RevertProcessor, BranchProcessor, MergeProcessor, ConflictResolver, AccessControlProcessor: Các thành phần xử lý cụ thể cho các yêu cầu khác nhau.

AccessControl: Quản lý quyền truy cập của người dùng.

Các mũi tên thể hiện dòng dữ liệu giữa các thành phần của hệ thống.

## Mô hình thực thể kết hợp
```
erDiagram
    USER {
        int id
        string username
        string email
        string password_hash
    }

    REPOSITORY {
        int id
        string name
        string description
        date created_at
        int owner_id
    }

    BRANCH {
        int id
        string name
        int repository_id
        int head_commit_id
    }

    COMMIT {
        int id
        string hash
        string message
        date timestamp
        int author_id
        int repository_id
    }

    FILE {
        int id
        string path
        string content
        int commit_id
    }

    ACCESS_CONTROL {
        int id
        int user_id
        int repository_id
        string access_level
    }

    %% Các mối kết hợp giữa các thực thể
    USER ||--o{ REPOSITORY : "owns"
    USER ||--o{ COMMIT : "makes"
    USER ||--o{ ACCESS_CONTROL : "has"

    REPOSITORY ||--o{ BRANCH : "has"
    REPOSITORY ||--o{ COMMIT : "contains"

    BRANCH ||--o{ COMMIT : "points to"

    COMMIT ||--o{ FILE : "contains"

    ACCESS_CONTROL }o--|| REPOSITORY : "applies to"
```
```mermaid
erDiagram
    USER {
        int id
        string username
        string email
        string password_hash
    }

    REPOSITORY {
        int id
        string name
        string description
        date created_at
        int owner_id
    }

    BRANCH {
        int id
        string name
        int repository_id
        int head_commit_id
    }

    COMMIT {
        int id
        string hash
        string message
        date timestamp
        int author_id
        int repository_id
    }

    FILE {
        int id
        string path
        string content
        int commit_id
    }

    ACCESS_CONTROL {
        int id
        int user_id
        int repository_id
        string access_level
    }

    %% Các mối kết hợp giữa các thực thể
    USER ||--o{ REPOSITORY : "owns"
    USER ||--o{ COMMIT : "makes"
    USER ||--o{ ACCESS_CONTROL : "has"

    REPOSITORY ||--o{ BRANCH : "has"
    REPOSITORY ||--o{ COMMIT : "contains"

    BRANCH ||--o{ COMMIT : "points to"

    COMMIT ||--o{ FILE : "contains"

    ACCESS_CONTROL }o--|| REPOSITORY : "applies to"
```
### Giải thích
USER: Thực thể đại diện cho người dùng với các thuộc tính như id, username, email, và password_hash.

REPOSITORY: Thực thể đại diện cho kho lưu trữ với các thuộc tính như id, name, description, created_at, và owner_id.

BRANCH: Thực thể đại diện cho nhánh với các thuộc tính như id, name, repository_id, và head_commit_id.

COMMIT: Thực thể đại diện cho phiên bản commit với các thuộc tính như id, hash, message, timestamp, author_id, và repository_id.

FILE: Thực thể đại diện cho tệp tin với các thuộc tính như id, path, content, và commit_id.

ACCESS_CONTROL: Thực thể đại diện cho quyền truy cập với các thuộc tính như id, user_id, repository_id, và access_level.
### Các mối kết hợp
Người dùng (USER) sở hữu nhiều kho lưu trữ (REPOSITORY).

Người dùng (USER) thực hiện nhiều phiên bản commit (COMMIT).

Người dùng (USER) có nhiều quyền truy cập (ACCESS_CONTROL).

Kho lưu trữ (REPOSITORY) có nhiều nhánh (BRANCH).

Kho lưu trữ (REPOSITORY) chứa nhiều phiên bản commit (COMMIT).

Nhánh (BRANCH) trỏ đến nhiều phiên bản commit (COMMIT).

Phiên bản commit (COMMIT) chứa nhiều tệp tin (FILE).

Quyền truy cập (ACCESS_CONTROL) áp dụng cho nhiều kho lưu trữ (REPOSITORY).
## Lưu đồ giải thuật
```
flowchart TD
    %% Terminal
    start([Bắt đầu]) --> user_input[[Nhận yêu cầu từ người dùng]]

    %% Decision cho các loại yêu cầu
    user_input -->|Yêu cầu commit| process_commit[Xử lý commit]
    user_input -->|Yêu cầu revert| process_revert[Xử lý revert]
    user_input -->|Yêu cầu tạo nhánh| process_create_branch[Xử lý tạo nhánh]
    user_input -->|Yêu cầu hợp nhất| process_merge[Xử lý hợp nhất]
    user_input -->|Yêu cầu khác| check_access_control[Kiểm tra quyền truy cập]

    %% Xử lý commit
    process_commit --> check_access_commit[Kiểm tra quyền truy cập cho commit]
    check_access_commit -->|Quyền hợp lệ| update_version_commit[Cập nhật cơ sở dữ liệu phiên bản]
    check_access_commit -->|Quyền không hợp lệ| access_denied[Quyền bị từ chối]
    update_version_commit --> update_branch_commit[Cập nhật cơ sở dữ liệu nhánh]
    update_branch_commit --> output_commit[[Trả kết quả cho người dùng]]
    
    %% Xử lý revert
    process_revert --> check_access_revert[Kiểm tra quyền truy cập cho revert]
    check_access_revert -->|Quyền hợp lệ| update_version_revert[Cập nhật cơ sở dữ liệu phiên bản]
    check_access_revert -->|Quyền không hợp lệ| access_denied
    update_version_revert --> output_revert[[Trả kết quả cho người dùng]]
    
    %% Xử lý tạo nhánh
    process_create_branch --> check_access_branch[Kiểm tra quyền truy cập cho tạo nhánh]
    check_access_branch -->|Quyền hợp lệ| update_branch_create[Cập nhật cơ sở dữ liệu nhánh]
    check_access_branch -->|Quyền không hợp lệ| access_denied
    update_branch_create --> output_create_branch[[Trả kết quả cho người dùng]]
    
    %% Xử lý hợp nhất
    process_merge --> check_access_merge[Kiểm tra quyền truy cập cho hợp nhất]
    check_access_merge -->|Quyền hợp lệ| update_version_merge[Cập nhật cơ sở dữ liệu phiên bản]
    update_version_merge --> resolve_conflict[Giải quyết xung đột]
    resolve_conflict --> update_version_conflict[Cập nhật cơ sở dữ liệu phiên bản sau xung đột]
    check_access_merge -->|Quyền không hợp lệ| access_denied
    update_version_conflict --> output_merge[[Trả kết quả cho người dùng]]

    %% Kiểm tra quyền truy cập cho các yêu cầu khác
    check_access_control --> verify_user[Xác minh người dùng]
    verify_user -->|Người dùng hợp lệ| process_request[Xử lý yêu cầu khác]
    verify_user -->|Người dùng không hợp lệ| access_denied

    %% Quyền bị từ chối
    access_denied --> output_access_denied[[Trả kết quả từ chối quyền truy cập]]

    %% Connectors
    output_commit --> finish([Kết thúc])
    output_revert --> finish
    output_create_branch --> finish
    output_merge --> finish
    output_access_denied --> finish

    %% Style
    style start fill:#e0e0e0,stroke:#000,stroke-width:2px;
    style finish fill:#e0e0e0,stroke:#000,stroke-width:2px;
    style user_input fill:#fff,stroke:#000,stroke-width:1px;
    style access_denied fill:#ffcccc,stroke:#d9534f,stroke-width:2px;
```
```mermaid
flowchart TD
    %% Terminal
    start([Bắt đầu]) --> user_input[[Nhận yêu cầu từ người dùng]]

    %% Decision cho các loại yêu cầu
    user_input -->|Yêu cầu commit| process_commit[Xử lý commit]
    user_input -->|Yêu cầu revert| process_revert[Xử lý revert]
    user_input -->|Yêu cầu tạo nhánh| process_create_branch[Xử lý tạo nhánh]
    user_input -->|Yêu cầu hợp nhất| process_merge[Xử lý hợp nhất]
    user_input -->|Yêu cầu khác| check_access_control[Kiểm tra quyền truy cập]

    %% Xử lý commit
    process_commit --> check_access_commit[Kiểm tra quyền truy cập cho commit]
    check_access_commit -->|Quyền hợp lệ| update_version_commit[Cập nhật cơ sở dữ liệu phiên bản]
    check_access_commit -->|Quyền không hợp lệ| access_denied[Quyền bị từ chối]
    update_version_commit --> update_branch_commit[Cập nhật cơ sở dữ liệu nhánh]
    update_branch_commit --> output_commit[[Trả kết quả cho người dùng]]
    
    %% Xử lý revert
    process_revert --> check_access_revert[Kiểm tra quyền truy cập cho revert]
    check_access_revert -->|Quyền hợp lệ| update_version_revert[Cập nhật cơ sở dữ liệu phiên bản]
    check_access_revert -->|Quyền không hợp lệ| access_denied
    update_version_revert --> output_revert[[Trả kết quả cho người dùng]]
    
    %% Xử lý tạo nhánh
    process_create_branch --> check_access_branch[Kiểm tra quyền truy cập cho tạo nhánh]
    check_access_branch -->|Quyền hợp lệ| update_branch_create[Cập nhật cơ sở dữ liệu nhánh]
    check_access_branch -->|Quyền không hợp lệ| access_denied
    update_branch_create --> output_create_branch[[Trả kết quả cho người dùng]]
    
    %% Xử lý hợp nhất
    process_merge --> check_access_merge[Kiểm tra quyền truy cập cho hợp nhất]
    check_access_merge -->|Quyền hợp lệ| update_version_merge[Cập nhật cơ sở dữ liệu phiên bản]
    update_version_merge --> resolve_conflict[Giải quyết xung đột]
    resolve_conflict --> update_version_conflict[Cập nhật cơ sở dữ liệu phiên bản sau xung đột]
    check_access_merge -->|Quyền không hợp lệ| access_denied
    update_version_conflict --> output_merge[[Trả kết quả cho người dùng]]

    %% Kiểm tra quyền truy cập cho các yêu cầu khác
    check_access_control --> verify_user[Xác minh người dùng]
    verify_user -->|Người dùng hợp lệ| process_request[Xử lý yêu cầu khác]
    verify_user -->|Người dùng không hợp lệ| access_denied

    %% Quyền bị từ chối
    access_denied --> output_access_denied[[Trả kết quả từ chối quyền truy cập]]

    %% Connectors
    output_commit --> finish([Kết thúc])
    output_revert --> finish
    output_create_branch --> finish
    output_merge --> finish
    output_access_denied --> finish

    %% Style
    style start fill:#e0e0e0,stroke:#000,stroke-width:2px;
    style finish fill:#e0e0e0,stroke:#000,stroke-width:2px;
    style user_input fill:#fff,stroke:#000,stroke-width:1px;
    style access_denied fill:#ffcccc,stroke:#d9534f,stroke-width:2px;
```
### Giải thích
#### Bắt đầu và Nhận yêu cầu từ người dùng
Bắt đầu tại điểm "Bắt đầu" và nhận yêu cầu từ người dùng qua "Nhận yêu cầu từ người dùng"
#### Quyết định loại yêu cầu:
##### Lưu đồ kiểm tra loại yêu cầu từ người dùng và phân nhánh xử lý cho từng loại
Yêu cầu commit (Gửi thay đổi lên kho lưu trữ)

Yêu cầu revert (Hoàn tác thay đổi)

Yêu cầu tạo nhánh (Tạo nhánh mới)

Yêu cầu hợp nhất (Hợp nhất các nhánh)

Yêu cầu khác (Các yêu cầu không xác định khác)
#### Xử lý commit
##### Kiểm tra quyền truy cập cho commit
Nếu quyền hợp lệ: Cập nhật cơ sở dữ liệu phiên bản và cơ sở dữ liệu nhánh, sau đó trả kết quả cho người dùng.

Nếu quyền không hợp lệ: Trả kết quả từ chối quyền truy cập cho người dùng.
#### Xử lý revert
##### Kiểm tra quyền truy cập cho revert
Nếu quyền hợp lệ: Cập nhật cơ sở dữ liệu phiên bản, sau đó trả kết quả cho người dùng.

Nếu quyền không hợp lệ: Trả kết quả từ chối quyền truy cập cho người dùng.
#### Xử lý tạo nhánh

##### Kiểm tra quyền truy cập cho tạo nhánh
Nếu quyền hợp lệ: Cập nhật cơ sở dữ liệu nhánh, sau đó trả kết quả cho người dùng.

Nếu quyền không hợp lệ: Trả kết quả từ chối quyền truy cập cho người dùng.
#### Xử lý hợp nhất
##### Kiểm tra quyền truy cập cho hợp nhất
Nếu quyền hợp lệ: Cập nhật cơ sở dữ liệu phiên bản và giải quyết xung đột nếu có, sau đó cập nhật lại cơ sở dữ liệu phiên bản và trả kết quả cho người dùng.

Nếu quyền không hợp lệ: Trả kết quả từ chối quyền truy cập cho người dùng.
#### Kiểm tra quyền truy cập cho các yêu cầu khác
##### Xác minh người dùng
Nếu người dùng hợp lệ: Xử lý các yêu cầu khác và trả kết quả.

Nếu người dùng không hợp lệ: Trả kết quả từ chối quyền truy cập cho người dùng.
#### Kết thúc
Tất cả các quy trình xử lý đều dẫn đến điểm kết thúc "Kết thúc"
