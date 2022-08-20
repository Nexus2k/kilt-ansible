# kilt-ansible
Ansible scripts to deploy a kilt collator using docker.

## Please Nominate or Tip Me!

* Polkadot: [12Dw4SzhsxX3fpDiLUYXm9oGbfxcbg1Peq67gc5jkkEo1TKr](https://polkadot.subscan.io/waiting/12Dw4SzhsxX3fpDiLUYXm9oGbfxcbg1Peq67gc5jkkEo1TKr) (🍁 HIGH/STAKE 🥩/HEL1 - 2.69% commission)
* Polkadot: [12bget8jJWnyjqYPiCwkXZjDh5tDBkta1WUcDYyndbXVDmQ1](https://polkadot.subscan.io/waiting/12bget8jJWnyjqYPiCwkXZjDh5tDBkta1WUcDYyndbXVDmQ1) (🍁 HIGH/STAKE 🥩/ZRH1 - 0.69% commission)
* Kusama: [DbRgw96nMQcFEFZWTLd6LSPNdh8u3NBuUDfAhDmB6UU8cJC](https://thousand-validators.kusama.network/#/leaderboard/DbRgw96nMQcFEFZWTLd6LSPNdh8u3NBuUDfAhDmB6UU8cJC) (🍁 HIGH/STAKE 🥩/HEL1 - 4.20% commission)
* Kusama: [HQuPha82sRy91zZn73XRGJVV3ernBh5HZKftUcoDT8CSUwK](https://thousand-validators.kusama.network/#/leaderboard/HQuPha82sRy91zZn73XRGJVV3ernBh5HZKftUcoDT8CSUwK) (🍁 HIGH/STAKE 🥩/ZRH1 - 10% commission)
* Zeitgeist: [dE1AAA5AxmbcvXLfLKwV8razVm5BxXVNpsgkgKLMB8jC4epbt](https://zeitgeist.subscan.io/account/dE1AAA5AxmbcvXLfLKwV8razVm5BxXVNpsgkgKLMB8jC4epbt) (🍁 HIGH/STAKE 🥩/PRG1 - 20% network defined commission)

## Getting started

Prerequisits:

*  VPS or dedicated server meeting the Kilt requirements
*  ansible (2.11.6+)
*  ansible-playbook (2.11.6+)

First copy the `hosts.ini-sample` to `hosts.ini` then:

1.  Rename the `my_node_*` entry to a name you like, also replace the `<IP of your server>` with the actual IP of your server
1.  Copy/Rename `host_vars/my_node_*.yml` file to the same name you just called the host in the `hosts.ini` file.
1.  Update the values in the `host_vars/my_node_*.yml` files, you always want to:
  1.  Update the `node_name` this is the node name your nodes will show up with in the Telemetry dashboards

## Deployment

If you feel adventerous you can deploy the whole server using:

```
$ ansible-playbook -i hosts.ini all.yml
```

This will execute the following roles:

* machine-setup
  *  Setup Docker
  *  Setup Journald
  *  Setup motd (message of the day)
* kilt-node
  *  Starts the zeitgeist nodes as docker containers
* watchtower (not run automatically, run using `ansible-playbook -i hosts.ini setup_watchtower.yml`)
  *  Sets up a watchtower container which monitors upstream container image repositories for new releases and upgrades automatically.

You can also run the individual roles using the `setup_*.yml` playbooks instead of `all.yml`.

### Kilt upgrades

To upgrade to the latest Kilt version you can simply restart the containers using the `setup_node.yml` playbook.

## Notes

Monitoring is not yet covered in these ansible playbooks. Do NOT run a Kilt collator w/o proper monitoring.

You should not use `root` user on the server, instead replace the `ansible_user` field in `hosts.ini` with an unpriviledged user (which has docker rights).
