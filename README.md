This was a mirror of http://www.vim.org/scripts/script.php?script_id=1425

and has some updated keyword highlighting

# updating the syntax highlighting form man pages
## longopts "iptablesLongParam"

```sh
man iptables-extensions | grep -oP -- '--\S*' | sed 's/[,.]//g' | sort | uniq | sed ':a;N;$!ba;s/\n/ /g' | fold -s | sed -e 's/^/   \\ /'
man iptables | grep -oP -- '--\S*' | grep -Pv -- '--(append|delete|insert|replace|list|flush|zero|new-chain|delete-chain|policy|rename-chain)' | \
    sed 's/[,.]//g' | sort | uniq | sed ':a;N;$!ba;s/\n/ /g' | fold -s | sed -e 's/^/   \\ /'
man xtables-addons | grep -oP -- '--\S*' | sed 's/[,.]//g' | sort | uniq | sed ':a;N;$!ba;s/\n/ /g' | fold -s | sed -e 's/^/   \\ /'
```

## modules and targets "iptablesTarget" and "iptablesModuleName"
you could use this:

```sh
#!/bin/bash
declare -a modules
declare -a targets
for i in $(
    zgrep -oP '(?<=\.SS )\S*' /usr/share/man/man8/iptables-extensions.8.gz
    zgrep -oP '(?<=\.SS )\S*' /usr/share/man/man8/xtables-addons.8.gz
); \
do if [[ "$i" =~ [A-Z] ]]; then
    targets+=("$i")
else
    modules+=("$i")
fi

done

echo modules:
echo ${modules[@]} | fold -s | sed -e 's/^/   \\ /'
echo targets:
echo ${targets[@]} | fold -s | sed -e 's/^/   \\ /'
```

but it assumes that the man page is formatted correctly (hint: its not)
