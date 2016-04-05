"Debug iox" is workable at 15.6(1)T1 and also has output during GOS rebooting.

Please use "show platform guest-os" instead of ""show iox host list detail" to check the Guest OS's status. 
"show iox host list detail" is to check if GOS register to IOS. 
If it is not register correctly, please double check if "IPv6" has been enable at GE5.