## Система оценивания практических работ студентов с помощью LLM

### Архитектура
```mermaid
graph LR
    %% C4 style classes c4model.com %%
    classDef person fill:#08427b,stroke:black,color:white;
    classDef container fill:#1168bd,stroke:black,color:white;
    classDef database fill:#1168bd,stroke:black,color:white;
    classDef software fill:#1168bd,stroke:black,color:white;
    classDef existing fill:#999999,stroke:black,color:white;
    classDef boundary fill:white,stroke:black,stroke-width:2px,stroke-dasharray: 5 5;
    classDef frame fill:white,stroke:black;


    %% nodes %%
    GitHub["GitHub API"]:::existing
    Mistral["Mistral AI API"]:::existing
    Admin((Admin)):::person
    Student((Student)):::person
    WebApp("Admin Panel <br>[Container: Nginx + React]"):::container
    CoreAPI("Core API <br>[Container: FastAPI]"):::container
    Worker("LLM Grader <br>[Container: FastStream]"):::container
    GitHubGateway("GitHub Gateway <br>[Container: FastAPI]"):::container
    CoreDB[("Core Data <br>[Container: PostgreSQL]")]:::database
    S3[("Submissions <br>[Container: Minio]")]:::database
    Events(["Events <br>[Container: Kafka Topic]"]):::container
    TGBot("Student Bot <br>[Container: Aiogram]"):::container

    %% connections and boundaries %%
    
    subgraph Legend [Containers]
        Student-.->|Uses| TGBot
        Admin-.->|Uses| WebApp
        
        subgraph Boundary["Boundary: System"]
            WebApp-.->|"Makes requests <br> [HTTP/HTTPS]"| CoreAPI
            CoreAPI-.->|"Makes requests  <br> [HTTP/HTTPS]"| GitHubGateway
            CoreAPI-.->|"Makes requests  <br> [TCP]"| CoreDB
            CoreAPI-.->|"Makes requests <br> [HTTP/HTTPS]"| S3

            Worker-.->|"Makes requestts <br> [HTTP/HTTPS]"| CoreAPI
            Worker-.->|"Makes requests <br> [HTTP/HTTPS]"| S3

            CoreAPI-.->|"Pushes event  <br> [TCP]"| Events
            TGBot-.->|"Pulls event <br> [TCP]"| Events
            Worker-.->|"Pulls event <br> [TCP]"| Events
            GitHubGateway-.->|"Pushes/pulls event <br> [TCP]"| Events

            
            TGBot-.->|"Makes requests <br> [HTTP/HTTPS]"| GitHubGateway
            TGBot-.->|"Makes requests <br> [HTTP/HTTPS]"| CoreAPI

        end
        class Boundary boundary
        
        GitHubGateway-.->|"Makes requests <br> [HTTP/HTTPS]"| GitHub
        Worker-.->|"Makes requests <br> [HTTP/HTTPS]"| Mistral
    end
    class Legend frame
```

### Скриншоты

#### Задачи

![image](https://github.com/user-attachments/assets/cef0a591-d2bf-4dc5-9842-03761f083e11)
![image](https://github.com/user-attachments/assets/adc5baae-2878-41b6-a00e-ced0915b83f3)
![image](https://github.com/user-attachments/assets/fb89da92-67dc-43c6-8f13-0ac27c12a708)
![image](https://github.com/user-attachments/assets/a2f95c84-969c-427e-b908-6fb327c14032)


#### Жалобы/вопросы студентов

![image](https://github.com/user-attachments/assets/c0c18c66-70b1-40e4-adb5-f9b59e0d77bb)
![image](https://github.com/user-attachments/assets/29d9e83c-9ac6-4713-af87-ae1b06e503f4)

#### Студенты

![image](https://github.com/user-attachments/assets/29ee71b8-7d94-4416-a8dd-2611ff72bc69)

