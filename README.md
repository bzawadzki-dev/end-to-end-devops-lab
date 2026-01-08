# End-to-End DevOps Lab: Infrastructure Automation and Monitoring

## Project Overview
Ten projekt demonstruje pełny proces wdrażania infrastruktury i aplikacji w chmurze AWS przy użyciu podejścia Infrastructure as Code (IaC) oraz Configuration as Code (CaC). System obejmuje automatyczny provisioning serwera, konteneryzację aplikacji oraz pełny stos monitoringu.

## Tech Stack
* **Cloud Provider:** AWS (EC2, Security Groups)
* **Infrastructure as Code:** Terraform
* **Configuration Management:** Ansible
* **Containerization:** Docker
* **Monitoring & Observability:** Prometheus, Grafana, Node Exporter
* **Web Server:** Nginx

## Architecture
1. **Infrastructure Layer:** Terraform definiuje zasoby w AWS, w tym reguły zapory sieciowej (Security Groups) umożliwiające dostęp przez porty 22 (SSH), 80 (HTTP), 3000 (Grafana) oraz 9090 (Prometheus).
2. **Configuration Layer:** Ansible automatyzuje instalację silnika Docker na systemie Ubuntu oraz konfiguruje parametry sieciowe i uprawnienia.
3. **Application Layer:** Aplikacja jest budowana jako obraz Docker, przechowywana w Docker Hub i wdrażana jako kontener na instancji EC2.
4. **Monitoring Layer:** Stos Prometheus/Grafana zbiera metryki systemowe za pomocą Node Exportera, umożliwiając wizualizację obciążenia serwera w czasie rzeczywistym.

## Project Structure
- `/terraform`: Pliki konfiguracyjne HCL do zarządzania zasobami AWS.
- `/ansible`: Playbooki oraz konfiguracje YAML dla monitoringu i środowiska Docker.
- `/app`: Kod źródłowy aplikacji wraz z instrukcją Dockerfile.

## Troubleshooting & Key Lessons
W trakcie realizacji projektu rozwiązano kluczowe problemy konfiguracyjne:
- **Port Forwarding:** Skorygowano mapowanie portów Docker (z 80:8080 na 80:80), aby dopasować je do domyślnej konfiguracji serwera Nginx wewnątrz kontenera.
- **Monitoring Networking:** Rozwiązano problem połączenia między kontenerami poprzez zastąpienie adresu `localhost` publicznym IP serwera w konfiguracji Data Source Grafany.
- **Security Groups:** Zidentyfikowano i poprawiono błędy składniowe w pliku `main.tf`, co pozwoliło na poprawne otwarcie portów dla usług monitoringu.

## How to Run
1. Zainicjalizuj infrastrukturę: `cd terraform && terraform apply`
2. Skonfiguruj dostęp Ansible w pliku `inventory.ini`.
3. Uruchom konfigurację środowiska: `ansible-playbook setup_monitoring.yml`
