+++
title = "Running Emacs in server mode"
author = ["Pep Turró Mauri"]
tags = ["emacs"]
draft = true
+++

Note: taken from [2022-11-24]({{< relref "#d41d8c" >}}) notes, needs improvement.

I have been running Emacs in server mode for a while. So far, my workflow was:

1.  start emacs
2.  `M-x server-start`

Following the blog post (<a href="#citeproc_bib_item_1">Schwartzmeyer 2021</a>), from now on I will try
running Emacs in server mode, started by systemd:

```sh
systemctl --user enable --now emacs
journalctl --user-unit emacs
```


## Fixing the systemd unit {#fixing-the-systemd-unit}

Enabling the systemd unit initially failed:

```text
pep@uio ~ $ systemctl --user enable emacs
Failed to enable unit: Unit file /home/pep/.config/systemd/user/default.target.wants/emacs.service does not exist.
```

Quickly fixed with:

```text
pep@uio user (master) $ mkdir -p ~/.config/systemd/user/default.target.wants
pep@uio user (master) $ systemctl --user enable emacs
Created symlink /home/pep/.config/systemd/user/default.target.wants/emacs.service → /usr/lib/systemd/user/emacs.service.
```

with the following contents:

```toml
[Unit]
Description=Emacs text editor
Documentation=info:emacs man:emacs(1) https://gnu.org/software/emacs/

[Service]
Type=notify
ExecStart=/usr/bin/emacs --fg-daemon

# Emacs will exit with status 15 after having received SIGTERM, which
# is the default "KillSignal" value systemd uses to stop services.
SuccessExitStatus=15

# The location of the SSH auth socket varies by distribution, and some
# set it from PAM, so don't override by default.
# Environment=SSH_AUTH_SOCK=%t/keyring/ssh
Restart=on-failure

[Install]
WantedBy=default.target
```


## Safe shutdown {#safe-shutdown}

Then, what about open files when closing?

Building upon [EmacsWiki docs for systemd](https://www.emacswiki.org/emacs/EmacsAsDaemon#h5o-2) I believe I should modify the systemd unit to include something like:

```toml
ExecStop=/usr/bin/emacsclient --eval "(save-buffers-kill-emacs)"
```

but I need to finalize some doubts, like setting `confirm-kill-process`.


## Checking status {#checking-status}

After starting it with `systemctl --user start emacs`:

```text
pep@uio ~ $ systemctl --user status emacs
● emacs.service - Emacs text editor
     Loaded: loaded (/usr/lib/systemd/user/emacs.service; disabled; vendor preset: disabled)
     Active: active (running) since Thu 2022-11-24 12:40:42 CET; 21min ago
       Docs: info:emacs
             man:emacs(1)
             https://gnu.org/software/emacs/
   Main PID: 43673 (emacs)
      Tasks: 8 (limit: 38145)
     Memory: 516.1M
        CPU: 44.767s
     CGroup: /user.slice/user-1000.slice/user@1000.service/app.slice/emacs.service
             ├─ 43673 /usr/bin/emacs --fg-daemon
             ├─ 43677 /home/pep/.emacs.d/elpa/emacsql-sqlite-20221024.1455/sqlite/emacsql-sqlite /home/pep/.emacs.d/org-roam.db
             ├─ 43710 /home/pep/.emacs.d/elpa/pdf-tools-20221007.1404/epdfinfo
             ├─ 43946 /usr/bin/mu server
             └─ 46975 /usr/bin/sh -i

de nov. 24 12:40:42 uio systemd[2978]: Started emacs.service - Emacs text editor.
```


## Updating client configuration {#updating-client-configuration}

I have updated my i3wm configuration so that `S-b` will create a new emacs
frame:

```text
$ grep -i emacs .config/i3/config
# open a new emacs frame
bindsym $mod+b exec --no-startup-id emacsclient -c
```

So, now, opening Emacs after logging in is just one keyboard shortcut
away.

Also updated aliases related to `emacsclient`:

```text
alias e='emacsclient'
alias et='emacsclient -t'  # for staying in the terminal
```

<style>.csl-entry{text-indent: -1.5em; margin-left: 1.5em;}</style><div class="csl-bib-body">
  <div class="csl-entry"><a id="citeproc_bib_item_1"></a>Schwartzmeyer, Andy. 2021. “Emacs on an iPad.” <a href="https://andschwa.com/posts/emacs-on-an-ipad/">https://andschwa.com/posts/emacs-on-an-ipad/</a>.</div>
</div>
