## Openvpn server on your Raspberry PI

From wikipedia:
> OpenVPN is an open source software application that implements virtual private network (VPN) techniques for creating secure point-to-point or site-to-site connections in routed or bridged configurations and remote access facilities. It uses a custom security protocol that utilizes SSL/TLS for key exchange.

What that means is that openvpn will allow you to safely and securely route your internet traffic through an untrusted network to a trusted one. It does this by encrypting your traffic so nobody can read it until it goes out from your openvpn server.

There are some benefits to this:

1. Prevent others from snooping on your traffic.
2. Access websites that are blocked by your work, school, or oppressive governments.
3. Access assets on your home network from anywhere.

This auto-install script will turn your raspberry pi into an openvpn server so you can browse the internet safely and securely.

Then why use it? Because sometimes you end up on insecure networks (think starbucks, stadiums, etc). This will protect your privacy in those situations. It will not prevent people from finding you if you are stupid and do something illegal.

## Running the installer
The auto-installer is completely automated and can be run directly from the web.

From your raspberry pi:
```bash
# Set the user
$ OPENVPN_USER='stephen.openvpn.local'

# Run the installer
$ curl "https://raw.githubusercontent.com/stephen-mw/raspberrypi-openvpn-autoinstall/master/bootstrap" | sudo bash
```

Remember that you should always inspect these types of files before ever running them. You can download it locally and run it like so:
```bash
# Download the file
$ wget "https://raw.githubusercontent.com/stephen-mw/raspberrypi-openvpn-autoinstall/master/bootstrap"

# Make sure it's legit
$ less bootstrap

# Execute it
$ chmod +x bootstrap
$ sudo ./bootstrap stephen.openvpn.local
```

## What's the installer do
The installer script will download openvpn and generate all of the necessary root certificates for you. Then it will generate and sign a new certificate for a user. Lastly it will create the ovpn file and place it in ```/root/client_<some_user>```.

All you need to do is download that file into your openvpn client software and you'll be able to safely and securely connect to the host. 

## Testing
I've included a Vagrantfile for you to run tests. Simply clone the repo and then run:
```bash
$ vagrant up
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...
==> default: Running cleanup tasks for 'shell' provisioner...
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Importing base box 'wheezy64'...
...
==> default: Make sure that your firewall allows incoming UDP connections to port 1194.
==> default:
==> default: The last thing necessary is to securely copy the configuration file over to your
==> default: computer and then load it. The configuration file is located at:
==> default:
==> default:   /root/client_test.ovpn
```

The vagrant guest will be running the openvpn server. You can pull down the client file and connect to it locally.
