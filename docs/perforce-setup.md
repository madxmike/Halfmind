# Perforce Setup

The user creates the Helix server, the stream depot, and two streams with this
guide.
The target host is the Linux machine.
The design `unreal-agent-delivery` defines the names in this guide.

## Goal

One Helix server runs on the Linux machine.
One stream depot holds the mainline stream `//Game/Main`.
The stream depot holds the development stream `//Game/madxmike`.
The development stream has `//Game/Main` as its parent.
The user `madxmike` holds a password and a login ticket.

## Constraints

- The user MUST run the commands on the Linux machine.
- The user MUST have `sudo` access on the Linux machine.
- The stream depot MUST hold no release stream.
- The password MUST NOT enter the repository.
- The user MUST type the password at the prompt.
- The commands use Ubuntu 24.04 and the distribution name `noble`.
- The user MUST replace `noble` when the distribution differs.

## Names

| Name | Value |
| --- | --- |
| Host | the Linux machine |
| Server root | `/p4/Game` |
| Server port | `1666` |
| Depot | `Game` |
| Depot type | stream |
| Mainline stream | `//Game/Main` |
| Stream of the user | `//Game/madxmike` |
| Parent of the user stream | `//Game/Main` |
| User | `madxmike` |

## Step 1: Install the server

Command:

```bash
wget -qO - https://package.perforce.com/perforce.pubkey | gpg --dearmor | sudo tee /usr/share/keyrings/perforce.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/perforce.gpg] https://package.perforce.com/apt/ubuntu noble release" | sudo tee /etc/apt/sources.list.d/perforce.list
sudo apt-get update
sudo apt-get install -y p4-server
```

Expected result: The package installs the server, the `p4` client, and the
`p4dctl` utility.
The command `/opt/perforce/sbin/p4d -V` prints one version line.
An older package index uses the name `helix-p4d`.

## Step 2: Set the start mode

The start mode uses the `p4dctl` service.
The service runs `p4d` as a background daemon.

Command:

```bash
sudo mkdir -p /p4/Game
sudo chown perforce:perforce /p4/Game
sudo mkdir -p /etc/perforce/p4dctl.conf.d
sudo tee /etc/perforce/p4dctl.conf.d/Game.conf > /dev/null <<'EOF'
p4d Game
{
    Owner = perforce
    Execute = /opt/perforce/sbin/p4d
    Environment
    {
        P4ROOT = /p4/Game
        P4PORT = 1666
        PATH = /bin:/usr/bin:/usr/local/bin:/opt/perforce/sbin
    }
}
EOF
sudo p4dctl start Game
```

Expected result: The `Game` service starts with no error.
The command `sudo p4dctl status Game` prints the `Game` service.

## Step 3: Check the server

Command:

```bash
p4 -p localhost:1666 info
```

Expected result: The command prints the server address, the server root, and
the server version.
The command shows no password prompt.

## Step 4: Create the stream depot

Command:

```bash
p4 -p localhost:1666 depot -o -t stream Game | p4 -p localhost:1666 depot -i
```

Expected result: The command prints a depot spec with `Depot: Game`.
The spec holds `Type: stream`.

## Step 5: Create the mainline stream

Command:

```bash
p4 -p localhost:1666 stream -o -t mainline //Game/Main | p4 -p localhost:1666 stream -i
```

Expected result: The command prints a stream spec with `Type: mainline`.
The spec holds `Parent: none`.

## Step 6: Create the stream of the user

The development stream is a child of the mainline stream.

Command:

```bash
p4 -p localhost:1666 stream -o -t development -P //Game/Main //Game/madxmike | p4 -p localhost:1666 stream -i
```

Expected result: The command prints a stream spec with `Type: development`.
The spec holds `Parent: //Game/Main`.

## Step 7: Create the user

Command:

```bash
p4 -p localhost:1666 user -i -f <<'EOF'
User: madxmike
Email: madxmike@localhost
FullName: madxmike
EOF
```

Expected result: The command prints `User madxmike saved.`

## Step 8: Set the credential

Command:

```bash
p4 -p localhost:1666 -u madxmike passwd
```

Expected result: The command asks for the new password two times.
The command prints no other text.

Command:

```bash
p4 -p localhost:1666 -u madxmike login
```

Expected result: The command asks for the password one time.
The command prints `User madxmike logged in.`
The command writes a ticket to `~/.p4tickets`.

Command:

```bash
p4 -p localhost:1666 -u madxmike login -s
```

Expected result: The command prints `User madxmike ticket expires in ...`.

## Step 9: Verify the result

Command:

```bash
p4 -p localhost:1666 streams //Game/...
```

Expected result:

```text
Stream //Game/Main mainline none 'Main'
Stream //Game/madxmike development //Game/Main 'madxmike'
```

Command:

```bash
p4 -p localhost:1666 info
```

Expected result: The command prints the server data with no password prompt.
The stream list holds no release stream.
