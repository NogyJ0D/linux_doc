# Qué hacer luego de instalar un OS:

## Debian 12:

1. Agregar usuario a sudoers:

	1. Iniciar como root: "su -".

	2. Agregar usuario: "usermod -aG sudo {usuario}"

	3. Reiniciar el OS.

2. Configurar con Gnome Tweaks:

	1. Instalar gnome-tweaks.

	2. Configurar.

3. Instalar neofetch.

4. Instalar tilix:

	1. Configurar tilix.

	2. Agregar atajo de tilix.

5. Instalar flatpak y firefox (no esr):

	> Firefox-esr se elimina porque es una versión LTS de firefox, por lo que está mas desactualizada.

	1. Instalar flatpak: "sudo apt install flatpak".

		1. Si el DE es Gnome, instalar el complemento para el software center: "sudo apt install gnome-software-plugin-flatpak".

	2. Añadir el repositorio de flathub: "sudo flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo"

	3. Reiniciar sesión.

	4. Instalar firefox: "sudo flatpak install flathub org.mozilla.firefox".

	5. Desinstalar firefox-esr: "sudo apt remove firefox-esr --yes".

6. Agregar los repositorios non-free y contrib:

	> Esto para acceder a paquetes y aplicaciones propietary.

	1. Entrar al archivo de repositorios: "sudo nano /etc/apt/sources.list".

	2. Agregar a todos los repositorios la fuente non-free: "main {non-free} {contrib} non-free-firmware".

	3. Actualizar repositorios: "sudo apt update".

7. Instalar extras:

	* "sudo apt install git curl --yes".

	* MS core fonts:

		1. Activar los repositorios non-free y contrib.

		2. Ejecutar "sudo apt install ttf-mscorefonts-installer -y".

# Instalar software:

## ZSH / Oh My ZSH

* Debian 12:

	1. Instalar zsh (si no está): "sudo apt install zsh -y".

	2. Configurar como consola por defecto: "chsh -s $(which zsh)".

	3. Instalar Oh My ZSH: "sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"".

	4. Configurar OMZ:

		1. Descargar plugins:

			1. zsh-syntax-highlighting (colorea los comandos): "git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting".

			2. zsh-autosuggestions (sugiere comandos s/historial): "git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions"

		2. Entrar al archivo de config: "sudo nano .zshrc".

		3. Cambiar el tema: 'ZSH_THEME="steeef"'

		4. Agregar plugins: "plugins=(git history zsh-syntax-highlighting zsh-autosuggestions)"

## Docker Engine
* Debian 12:
	1. Limpiar docker:
		```sh
		for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
		```
	3. Instalar esenciales:
		```sh
		sudo apt install ca-certificates curl gnupg
		```
	4. Añadir la llave GPG:
		```sh
		sudo install -m 0755 -d /etc/apt/keyrings
		```
		```sh
		curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
		```
		```sh
		sudo chmod a+r /etc/apt/keyrings/docker.gpg
		```
	5. Agregar el repositorio:
		```sh
		echo \
			"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
			"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
			sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
		```
	1. Actualizar repositorios.
	2. Instalar docker:
		```sh
		sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
		```
	3. Probar la instalación:
		```sh
		sudo docker run --name hw hello-world
		```
## Kathara Lab:
 * Debian 12:

	> **Requisito: Docker Engine**
	1. Añadir la GPG Key:
		```sh
		sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 21805A48E6CBBA6B991ABE76646193862B759810
		```
	2. Como root ("su -"), agregar el repositorio:
		```sh
		echo "deb http://ppa.launchpad.net/katharaframework/kathara/ubuntu bionic main" > /etc/apt/sources.list.d/kathara.list
		```
		```sh
		# Para quitar el error de GPG:
		sudo apt-key export 2B759810 | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/kathara.gpg
		```
	3. Actualizar repositorios.
	4. Instalar Kathara:
		```sh
		sudo apt install kathara -y
		```
     * **Si da error porque falta libssl1.1:**
	   ```sh
	   sudo echo "deb [trusted=yes] http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list
	   ```
       ```sh
	   sudo apt update; sudo apt install libssl1.1 -y
	   ```
       ```sh
	   sudo apt install kathara -y
	   ```
       ```sh
	   sudo rm /etc/apt/sources.list.d/focal-security.list
	   ```
	5. Editar la config:
		```sh
		sudo nano .config/kathara.conf
		```
		1. Reemplazar la terminal xterm por bash.
		2. Poner "open_terminals" en false
	6. Comprobar si funciona:
		```sh
		sudo kathara check
		```
## GH CLI (Github CLI):
* Debian 12:
	1. Ejecutar:
		```sh
		type -p curl >/dev/null || (sudo apt update && sudo apt install curl -y)
		curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg | sudo dd of=/usr/share/keyrings/githubcli-archive-keyring.gpg \
		&& sudo chmod go+r /usr/share/keyrings/githubcli-archive-keyring.gpg \
		&& echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" | sudo tee /etc/apt/sources.list.d/github-cli.list > /dev/null \
		&& sudo apt update \
		&& sudo apt install gh -y
		```
	2. Iniciar sesión en github con el navegador predeterminado.
	3. Autenticarse en el cli:
		```sh
		gh auth login
		```
## Google Chrome

* Debian 12:
	```sh
 	sudo apt install apt-transport-https curl -y
  	```
 	```sh
	curl -fSsL https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor | sudo tee /usr/share/keyrings/google-chrome.gpg >> /dev/null
  	```
   	```sh
  	echo deb [arch=amd64 signed-by=/usr/share/keyrings/google-chrome.gpg] http://dl.google.com/linux/chrome/deb/ stable main | sudo tee /etc/apt/sources.list.d/google-chrome.list
	```
	> Si zsh no ejecutó el comando anterior, usar bash.
	```sh
	sudo apt update
	```
 	```sh
 	sudo apt install google-chrome-stable -y
	```

# Útiles
## Nano
* Mostrar número de línea en nano:
  	1. Entrar a nano.
  	2. Presionar las teclas alt + shift + 3.
