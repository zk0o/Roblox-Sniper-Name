# Python, Replit.
import requests
import threading
import os
import random
import string
import colorama
from colorama import Fore
import time

os.system("clear")

print(Fore.LIGHTGREEN_EX + """
██╗   ██╗███████╗███████╗██████╗     ███████╗███╗   ██╗██╗██████╗ ███████╗██████╗ 
██║   ██║██╔════╝██╔════╝██╔══██╗    ██╔════╝████╗  ██║██║██╔══██╗██╔════╝██╔══██╗
██║   ██║███████╗█████╗  ██████╔╝    ███████╗██╔██╗ ██║██║██████╔╝█████╗  ██████╔╝
██║   ██║╚════██║██╔══╝  ██╔══██╗    ╚════██║██║╚██╗██║██║██╔═══╝ ██╔══╝  ██╔══██╗
╚██████╔╝███████║███████╗██║  ██║    ███████║██║ ╚████║██║██║     ███████╗██║  ██║
 ╚═════╝ ╚══════╝╚══════╝╚═╝  ╚═╝    ╚══════╝╚═╝  ╚═══╝╚═╝╚═╝     ╚══════╝╚═╝  ╚═╝
""")

print('                 ', end="")
print(Fore.RED + '\033[4m' + '\033[1m' + 'USER-SNIPER By zk0o Discord [ADVANCED] V0.3' + '\033[0m')
print('                 ', end="")
print(Fore.RED + '\033[4m' + '\033[1m' + 'SOME VALIDS ARE INVALID DUE TO BANNED ACCOUNTS' + '\033[0m')
print('                 ', end="")
print(Fore.RED + '\033[4m' + '\033[1m' + 'WARN 3/4L 97% (+) Bug Chance And Send Fake Valid Name' + '\033[0m')
print('                 ', end="")
print(Fore.RED + '\033[4m' + '\033[1m' + 'Minimum letter (3), Max letter (20) if + or - Bug' + '\033[0m')
print('')

print(Fore.LIGHTCYAN_EX + '\033[1m' + "Minimum username length:", end="")
min_length_input = input(Fore.LIGHTRED_EX + " ")
while True:
    try:
        min_length = int(min_length_input)
        if min_length > 0:
            break
        else:
            print(Fore.RED + "Minimum length must be greater than 0.")
            min_length_input = input(Fore.LIGHTRED_EX + " ")
    except ValueError:
        print(Fore.RED + "Please enter a valid number for minimum username length.")
        min_length_input = input(Fore.LIGHTRED_EX + " ")

print(Fore.LIGHTCYAN_EX + '\033[1m' + "Maximum username length:", end="")
max_length_input = input(Fore.LIGHTRED_EX + " ")
while True:
    try:
        max_length = int(max_length_input)
        if max_length >= min_length:
            break
        else:
            print(Fore.RED + "Maximum length must be greater than or equal to minimum length.")
            max_length_input = input(Fore.LIGHTRED_EX + " ")
    except ValueError:
        print(Fore.RED + "Please enter a valid number for maximum username length.")
        max_length_input = input(Fore.LIGHTRED_EX + " ")

webhook_url = ""

def send_to_webhook(usernames):
    content = "\n".join([f"**{index} - {name}**" for index, name in usernames])
    payload = {"content": content}
    headers = {'Content-Type': 'application/json'}
    try:
        response = requests.post(webhook_url, json=payload, headers=headers)
        if response.status_code == 204:
            print(Fore.GREEN + f"Send Webhook Valid Names: {content}")
        else:
            print(Fore.RED + "Error sending to webhook!")
    except requests.exceptions.RequestException as e:
        print(Fore.RED + f"Error sending to webhook: {e}")

progress_file = "valid_usernames.txt"
valid_usernames = []
if os.path.exists(progress_file):
    with open(progress_file, "r") as file:
        valid_usernames = [line.strip() for line in file.readlines()]

def save_progress():
    with open(progress_file, "w") as file:
        file.write("\n".join(valid_usernames))

session = requests.Session()
session.headers.update({'User-Agent': 'Mozilla/5.0'})
session.verify = False

def do_request():
    global valid_usernames
    generated_usernames = []
    retry_count = 0
    while True:
        if len(generated_usernames) >= 40 or len(generated_usernames) >= 65:
            valid_usernames.extend(generated_usernames)
            save_progress()
            indexed_usernames = [(len(valid_usernames) - len(generated_usernames) + i + 1, name) for i, name in enumerate(generated_usernames)]
            send_to_webhook(indexed_usernames)
            generated_usernames.clear()

        username_length = random.randint(min_length, max_length)
        username = ''.join(random.choices(string.ascii_lowercase + string.digits, k=username_length))
        try:
            r = session.get(f"https://www.roblox.com/users/profile?username={username}")
            if r.url == "https://www.roblox.com/request-error?code=404":
                print(Fore.WHITE + "[", end="")
                print(Fore.GREEN + "+", end="")
                print(Fore.WHITE + "]", end="")
                print(Fore.BLUE + f" {username}")
                generated_usernames.append(username)
            else:
                print(Fore.WHITE + "[", end="")
                print(Fore.RED + "-", end="")
                print(Fore.WHITE + "]", end="")
                print(Fore.BLUE + f" {username}")
        except requests.exceptions.RequestException as e:
            retry_count += 1
            print(Fore.RED + f"Error verifying name {username}, trying again...{e}")
            time.sleep(2 * retry_count)
            if retry_count > 5:
                print(Fore.YELLOW + "Too many consecutive failures. Restarting...")
                retry_count = 0

threads = []
thread_limit = 10
for i in range(thread_limit):
    t = threading.Thread(target=do_request)
    t.daemon = True
    threads.append(t)

for i in range(thread_limit):
    time.sleep(1)
    threads[i].start()

while True:
    for i, t in enumerate(threads):
        if not t.is_alive():
            print(Fore.YELLOW + f"Thread {i} died. Restarting...")
            new_thread = threading.Thread(target=do_request)
            new_thread.daemon = True
            threads[i] = new_thread
            new_thread.start()
    time.sleep(10)
