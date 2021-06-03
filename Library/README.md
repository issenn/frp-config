
# Usage

```sh
man launchd.plist
```

- `system/[service-name]`  is privileged (runs services as root) and requires root for interaction
- `user/<uid>/[service-name]`  runs as that user, but does not require that the user be logged in
- `login/<asid>/[service-name]`
- `gui/<uid>/[service-name]`  runs as that user, but is only active when the user is logged in at the GUI
- `session/<asid>/[service-name]`
- `pid/<pid>/[service-name]`

Listing running Daemons/Agents.

```sh
launchctl list | grep "homebrew"
```

print

```sh
launchctl print <service-target> | <domain-target>

launchctl print gui/"$(id -u issenn)"
launchctl print gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv
launchctl print system
```

Dumps the service's definition, properties & metadata.

**WARNING**
The output of print is not officially structured, do not rely either on the format or on the information.

Loading a Daemon/Agent.

```sh
launchctl bootstrap <domain-target> <paths...>

launchctl bootstrap gui/"$(id -u issenn)" ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist

# This syntax was deprecated in Yosemite
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist
# launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist
```

The paths can be plist files, XPC bundles, or directories of them. Each plist or bundle is loaded into the specified domain.

Unloading a Daemon/Agent.

```sh
launchctl bootout <service-target> | <domain-target> <paths...>

launchctl bootout gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv

# This syntax was deprecated in Yosemite
launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist
# launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist
```

Unloads the specified service(s).

Unloading all Daemon/Agents.

```sh
launchctl bootout gui/"$(id -u issenn)"
```

**WARNING**  
The paths are actually optional, you can unload an entire domain, which you probably should not do.

enable a service.

```sh
launchctl enable <service-target> 

launchctl enable gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv
```

disable a service.

```sh
launchctl disable <service-target>

launchctl disable gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv
```

kill a service.

```sh
# <signal name or number>
launchctl kill <signame | signum> <service-target>

# Kill a service that has no plist file.
launchctl kill SIGTERM gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv
# or
launchctl kill SIGKILL gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv

# deprecated
launchctl remove homebrew.mxcl.frpc.setenv
```

Sends a signal to a service's process (without having to look it up manually).

reload a Daemon/Agent.

```sh
launchctl bootout gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv
launchctl enable gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv
launchctl bootstrap gui/"$(id -u issenn)" ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist

# This syntax was deprecated in Yosemite
launchctl unload ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist
```

debug

```sh
launchctl start homebrew.mxcl.frpc.setenv
launchctl stop homebrew.mxcl.frpc.setenv

launchctl start ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist
launchctl stop ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist
```

kickstart

```sh
launchctl kickstart <service-target>

launchctl kickstart -k gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv
```

Starts a service. -k will kill then restart existing instances.

Supposedly -p will print the service's pid (even if it's already started), doesn't seem to work for me.

---

frpc

```sh
launchctl stop homebrew.mxcl.frpc.unsetenv && launchctl bootout gui/"$(id -u issenn)"/homebrew.mxcl.frpc.unsetenv && launchctl enable gui/"$(id -u issenn)"/homebrew.mxcl.frpc.unsetenv && launchctl bootstrap gui/"$(id -u issenn)" ~/Library/LaunchAgents/homebrew.mxcl.frpc.unsetenv.plist && launchctl start homebrew.mxcl.frpc.unsetenv

launchctl stop homebrew.mxcl.frpc.setenv && launchctl bootout gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv && launchctl enable gui/"$(id -u issenn)"/homebrew.mxcl.frpc.setenv && launchctl bootstrap gui/"$(id -u issenn)" ~/Library/LaunchAgents/homebrew.mxcl.frpc.setenv.plist && launchctl start homebrew.mxcl.frpc.setenv
```

frps

```sh
launchctl stop homebrew.mxcl.frps.unsetenv && launchctl bootout gui/"$(id -u issenn)"/homebrew.mxcl.frps.unsetenv && launchctl enable gui/"$(id -u issenn)"/homebrew.mxcl.frps.unsetenv && launchctl bootstrap gui/"$(id -u issenn)" ~/Library/LaunchAgents/homebrew.mxcl.frps.unsetenv.plist && launchctl start homebrew.mxcl.frps.unsetenv

launchctl stop homebrew.mxcl.frps.setenv && launchctl bootout gui/"$(id -u issenn)"/homebrew.mxcl.frps.setenv && launchctl enable gui/"$(id -u issenn)"/homebrew.mxcl.frps.setenv && launchctl bootstrap gui/"$(id -u issenn)" ~/Library/LaunchAgents/homebrew.mxcl.frps.setenv.plist && launchctl start homebrew.mxcl.frps.setenv
```

---

used to control whether your job is to be kept continuously running or to let demand and conditions control the invocation.the default is false and therefore only demand will start the job. the value may be set to true to unconditionally keep the job alive. alternatively, a dictionary of conditions may be specified to selectively control whether launchd keeps a job alive or not. if multiple keys are provided,launchd ORs them,thus providing maximum flexibility to the job to refine the logic and stall if necessary.if launchd finds no reason to restart the job ,it falls back on demand based invocation. jobs that exit quickly and frequently when configured to be kept alive will be throttled to conserve system resources.
