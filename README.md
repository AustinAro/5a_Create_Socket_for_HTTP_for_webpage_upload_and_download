# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download

## Algorithm
1.Start the program.
2.Get the frame size from the user
3.To create the frame based on the user request.
4.To send frames to server from the client side.
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
6.Stop the program

## Program :
### File.py:
```
import socket

def send_request(host, port, request):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request.encode())
        response = s.recv(4096).decode(errors="ignore")
    return response

def upload_file(host, port, filename):
    # Just reading file and sending POST request (server will respond 404 or 405 which is OK)
    with open(filename, 'r') as file:
        file_data = file.read()
        content_length = len(file_data)

        request = (
            f"POST /upload HTTP/1.1\r\n"
            f"Host: {host}\r\n"
            f"Content-Length: {content_length}\r\n"
            f"Content-Type: text/plain\r\n\r\n"
            f"{file_data}"
        )

        response = send_request(host, port, request)
    return response

def download_file(host, port, filename):
    request = "GET / HTTP/1.1\r\nHost: info.cern.ch\r\n\r\n"


    response = send_request(host, port, request)

    # Save response body to file (not a real download)
    body = response.split("\r\n\r\n")[-1]
    with open("downloaded_output.txt", "w") as f:
        f.write(body)

    return "Downloaded (response saved)"

if __name__ == "__main__":
    host = "info.cern.ch"
    port = 80

    print("---- Upload Simulation ----")
    upload_response = upload_file(host, port, 'example.txt')
    print(upload_response)

    print("\n---- Download Simulation ----")
    download_response = download_file(host, port, 'example.txt')
    print(download_response)
```
## OUTPUT:

<img width="1261" height="708" alt="image" src="https://github.com/user-attachments/assets/84f81761-c83a-468a-a707-689edcb0e9c3" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
