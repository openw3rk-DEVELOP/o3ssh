import paramiko
import subprocess
import sys
import time
#                                                                                  
#                         ___     _      ____  _____ _____ _____ __    _____ _____ 
#   ___ ___ ___ ___ _ _ _|_  |___| |_   |    \|   __|  |  |   __|  |  |     |  _  |
#  | . | . | -_|   | | | |_  |  _| '_|  |  |  |   __|  |  |   __|  |__|  |  |   __|
#  |___|  _|___|_|_|_____|___|_| |_,_|  |____/|_____|\___/|_____|_____|_____|__|   
#      |_|                                                                         
# ---------------------------------------------------------------------------------------
# (c) openw3rk
# https://openw3rk.de 
# https://o3ssh.openw3rk.de
# develop@openw3rk.de
# o3ssh is open source, so it may be freely distributed, modified and used.
# ---------------------------------------------------------------------------------------
def install_paramiko():
    try:
        subprocess.run(["pip", "install", "paramiko"], check=True)
        import paramiko
        print("Paramiko wurde erfolgreich installiert.")
    except subprocess.CalledProcessError:
        print("Fehler bei der Installation von Paramiko. Stelle sicher, dass pip installiert ist und versuche es erneut.")
        sys.exit(1)

try:
    import paramiko
except ImportError:
    print("Paramiko ist nicht installiert. Installation wird durchgeführt...")
    install_paramiko()
except Exception as e:
    print("Fehler beim Importieren von Paramiko:", str(e))
    sys.exit(1)

def ssh_connect(hostname, port, username, password):
    try:
        client = paramiko.SSHClient()
        client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        client.connect(hostname, port=port, username=username, password=password)
        print("Erfolgreich mit SSH verbunden")

        shell = client.invoke_shell()
        
        time.sleep(1)
        if shell.recv_ready():
            welcome_message = shell.recv(4096).decode('utf-8')
            print(welcome_message, end='')
        
        if shell.recv_ready():
            prompt = shell.recv(4096).decode('utf-8').strip()
            print(prompt, end='')

        while True:
            command = input()  
            
            if command.lower() == 'exit':
                break
            
            shell.send(command + '\n')
            
            time.sleep(1)
            if shell.recv_ready():
                output = shell.recv(4096).decode('utf-8')
                print(output, end='')

            if shell.recv_ready():
                prompt = shell.recv(4096).decode('utf-8').strip()
                print(prompt, end='')

        client.close()
        print("Verbindung geschlossen.")
    except Exception as e:
        print("Verbindung FEHLGESCHLAGEN", str(e))
        sys.exit(1)

if __name__ == "__main__":
    print(r"""
       ____          _     
      |___ \        | |    
   ___  __) |___ ___| |__ 
  / _ \|__ </ __/ __| '_ \ 
 | (_) |__) \__ \__ \ | | |
  \___/____/|___/___/_| |_|
  https://o3ssh.openw3rk.de 
     develop@openw3rk.de
     """) 
    print("Willkommen zu o3ssh - (c) openw3rk ")
    print("**********************************\n")

    hostname = input("IP-Adresse oder Hostname: ")
    port = int(input("Port (Standard ist 22): ") or "22")  
    username = input("Benutzer: ")
    password = input("Passwort: ")
    
    ssh_connect(hostname, port, username, password)
