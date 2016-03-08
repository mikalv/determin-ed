# determin-ed
Create deterministic ed25519 keys from seedfile for openssh-key-v1 format

Problem
=======
SSH keys stored on mobile devices and laptop computers are typically password protected, so that if the device is stolen or compromised the attacker has to guess the password in order to use the key. Only raw computing power limits the rate of the attacker's guesses when the key is not stored in a TPM (the TPM should withstand attacks as well). Fortunately, there are new password strengthening mechanisms, which try to minimize the computing gap between the most powerful attacker and the weakest user device. Nevertheless, the attacker advantage can be estimated to exceed 30 bits[1] of password entropy, which requires lengthy passwords or inhuman wait times.

What if
=======
What if the attacker would not be able to confirm the key guesses without trying them on the target server?

 - The public key should not be at the device
 - Valid private key decryption should not have structure
 - OpenSSH public key query should be resistant to timing attacks

 This proof of concept tool is intended to solve the first two problems (don't know anything about the last issue).

 Usage
 =====
 Create a key seed file.
 Any tool that can output random garbage like ssh-keygen is fine for this.
ssh-keygen -t ed25519 -f keyseed
 Not the most elegant, but doesn't matter, got entropy.
 Use the deterministic-ed to create a deterministic SSH key from the seed file
deterministic-ed keyseed
cat id_new.pub
 Put the resulting public key (id_new.pub) to your target server. Delete both id_new* files.
 Automate a command to create your keys when connecting to target
 ProxyCommand determined ~/.ssh/id_rsa.pub -out=~/.ssh/id_new; exec socket %h %p && rm ~/.ssh/id_new*



[1] Give attacker more time and parallel GPUs worth of 1M$, shittiest hardware for the user and 0.05 s wait time  https://litecoin.info/Mining_hardware_comparison
