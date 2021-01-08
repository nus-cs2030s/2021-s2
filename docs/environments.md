# The CS2030S Programming Environment

## Java version

Java is a language that continues to evolve.  A new version is released every six months, and by the time we are done with CS2030S this semester, we will have Java 16.  For CS2030S, we will _only_ use Java 11, the most recent version with long-term support.

## Programming Servers

The school has provided a list of computing servers for you to use.  You can access them remotely via `ssh`, or secure shell.  The hosts are named `pe111`, `pe112`, ... , `pe120`.  (`pe` stands for "programming environment").  We will refer to these servers generally as the _PE hosts._

For this semester, the two servers `pe115` and `pe116` are not available.

You can choose which of the eight hosts to use.  You share the same home directory across all the hosts (this home directory, however, is different from that of `sunfire`).  If you notice that one host is crowded, you can use another host to spread out the load.

While you can complete the programming assignments on your computers, the practical exams are done in a controlled environment using servers similar to the PE hosts.  It is therefore advisable for you to familiarize yourself with accessing the PE servers via `ssh` and edit your program with either `vim`.

## Basic Requirements

1. You need to have an SoC Unix account.  If you do not have one, you can [apply for one online](https://mysoc.nus.edu.sg/~newacct/).

2. Once you have an account, you need to [activate your access to the PE hosts](https://mysoc.nus.edu.sg/~myacct/services.cgi), which is part of the SoC computer clusters.

3. You need a command-line `ssh` client.  Windows 10, macOS, and Linux users should already have it installed by default.

For older versions of Windows, such as those used in the SoC's programming labs, you can check out [XShell 6](https://www.netsarang.com/products/xsh_overview.html) (free for home/school use), or [PuTTY](https://www.putty.org).  These are GUI-based programs so the command line instructions below do not apply.

## The Command to SSH

Run:
```
ssh <username>@pe1xx.comp.nus.edu.sg
```

Replace `pe1xx` with the host you want to log into and `<username>` with your SoC Unix username.  Note that both are case sensitive.

I would do:
```
ssh ooiwt@pe115.comp.nus.edu.sg
```
to log into `pe115`.

After the command above, following the instructions on the screen.  The first time you ever connect to a PE host, you will be warned that you are connecting to a previously unknown host.  Say `yes`, and you will be prompted with your SoC Unix password.

## Accessing The PE Hosts from Outside SoC

The PE hosts can only be accessed from within the School of Computing networks.  To complete the lab at home and to complete the two practical assessments from home, you need to access the PE hosts from outside the SoC networks.  

There are several ways to do this, the simplest way is to tunnel through `sunfire`, and this is the recommended method, as no extra software is required.

### Option 1: Tunneling through Sunfire (Recommended for those in Singapore)

SoC's Sunfire (`sunfire.comp.nus.edu.sg`) is configured to allow your connection if it's originating from a local telco (See [more details here](https://dochub.comp.nus.edu.sg/cf/guides/unix/soc_unix_pass_your_direct_access_to_soc_unix_servers)).

There are two ways to achieve this, and in both ways, it appears to the PE hosts that Sunfire is the client.

Connect to sunfire at `sunfire.comp.nus.edu.sg` via command line `ssh` with the option `-t`.
```
ssh -t <username1>@sunfire.comp.nus.edu.sg ssh <username2>@pe1xx.comp.nus.edu.sg
```

Note that in normal situations, `username1` and `username2` are both your SoC Unix username.  For practical exams, you will be issued a special exam account to log into the PE hosts.  In this case, `username1` will be your SoC Unix username and `username2` will be your special exam account.

Students not in Singapore will need to access `sunfire` via SoC VPN.  In which case, Option 2 would be better.

### Option 2: Using SoC VPN (Recommended only for not in Singapore)

To set up the SOC Virtual Private Network (VPN), see [instruction here](https://dochub.comp.nus.edu.sg/cf/guides/network/vpn)).  The staff at `helpdesk@comp.nus.edu.sg` or the IT helpdesk in COM1, Level 1, will be able to help with you setting up if needed.

!!! note "SoC VPN vs NUS VPN"

    Note that SoC VPN is different from NUS VPN.  Connecting to NUS VPN only allows you access to the NUS internal network, but not the SoC internal network.

!!! warning "Windows 10 Users: FortiClient from Windows Store"

    Students have reported that running FortiClient downloaded from the Windows Store does not allow one to `ssh` from WSL to `sunfire` as expected.  Therefore, you should download and install For Windows 10 users, please download and install FortiClient VPN directly from [FortiClient's website](https://forticlient.com/downloads).

Once you are connected to SoC VPN, you can run
```
ssh <username>@pe1xx.comp.nus.edu.sg
```

## Setting up SSH Keys

Once you are comfortable with Unix, you can set up a pair of public/private keys for authentication.  

You can use
```
ssh-keygen -t rsa
```

to generate a pair of keys on your local computer.  Keep the private key `id_rsa` on your local machine in the hidden `~/.ssh` directory, and copy the public key `id_rsa.pub` to your home directory on the remote host you want to log into.  On this remote host, run
```
cat id_rsa.pub >> ~/.ssh/authorized_keys
```

Make sure that the permission for `.ssh` both on the local machine and on the remote host are set to `700` and the files `id_rsa` on the local machine and `authorized_keys` on the remote host are set to `600`.  Once set up, you need not enter your password every time you run `ssh` or `scp`.  

## Troubleshooting

If you get the following error:

1. `ssh: Could not resolve hostname pe1xx.comp.nus.edu.sg`

	`ssh` cannot recognize the name `pe1xx`, likely, you are not tunneling connected to the SoC VPN.

2. `Connection closed by 192.168.48.xxx port 22`

    You have connected to the PE host, but you are kicked out because you have no permission to use the host.

	Make sure you have activated your access to "SoC computer clusters" [here](https://mysoc.nus.edu.sg/~myacct/services.cgi)

3. `Permission denied, please try again`

    You did not enter the correct password or username.  Please use the username and password
of your SoC Unix account which you have created here: https://mysoc.nus.edu.sg/~newacct/.  

    Check that you have entered your username correctly.  It is _case sensitive_.

    If you have lost your password, go here: https://mysoc.nus.edu.sg/~myacct/iforgot.cgi

4. `ssh: connect to host sunfire.comp.nus.edu.sg port 22: Operation timed out`

	It means that you failed to connect to `sunfire` via `ssh`.  There could be two reasons for this: (i) `sunfire` or its ssh service is down; (ii) you are connecting via a network where `sunfire` is not accessible (such as outside Singapore).  

	The likelihood of (i) is small.  The more likely scenario is (ii), in which case, you should be able to solve it by connecting to SoC VPN.

5. `Could not chdir to home directory /home/o/ooiwt: Permission denied`

    This error means that you have successfully connected to the PE hosts, but you have no access to your home directory.

	This should not happen.  Please send an email with the above error message to `helpdesk@comp.nus.edu.sg`, include the PE hosts that you connected to with this error and your username.  The system administrator can reset the permission of your home directory for you.
