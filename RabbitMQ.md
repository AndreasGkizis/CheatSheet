Deleting queues
```bash
# Initialize counter
count=0

# Get total number of queues (minus the header)
total=$(rabbitmqctl list_queues -q name | tail -n +2 | wc -l)
echo "Found $total queues to delete"

for queue in $(rabbitmqctl list_queues -q name | tail -n +2); do
    rabbitmqctl delete_queue $queue
    ((count++))
    echo "Progress: $count/$total queues deleted"
done

echo "Operation complete. Deleted $count queues."
```
