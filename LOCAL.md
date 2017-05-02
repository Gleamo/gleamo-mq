# Setting Up MQ for Local Development

This will go through how to set up your local environment for
RabbitMQ. This will mainly focus on installing RabbitMQ and
configuring RabbitMQ for local development on a Mac OSX machine.

## Installing RabbitMQ

```sh
brew install rabbitmq
```

## Starting RabbitMQ

```sh
brew services start rabbitmq
```

## Configuring Users

By default, RabbitMQ comes with a guest user that can only
access the RabbitMQ service via localhost. That is fine for
running the integration tests for both the gleamo-device
and gleamo-server, but if you want to actually test the
gleamo-hardware, you will need to create a new user:

We recommend that you alias the RabbitMQ utilities. Add the following
to your `~/.bash_profile` or `~/.bashrc`:

```sh
export PATH="$PATH:/usr/local/sbin"
```

Remove the default user and add a new one:

```sh
rabbitmqctl delete_user guest
rabbitmqctl add_user gleamo gleamo
rabbitmqctl set_permissions gleamo "commands" ".*" ".*"
```

## Configuring Network

Add a configuration file in `/usr/local/etc/rabbitmq/rabbitmq.conf` with the following contents:

```erlang
[
  {rabbit, [
    {tcp_listeners, [
      {"::", 5672}
    ]}
  ]}
].
```

On OSX, you'll need to remove the following like from `/usr/local/etc/rabbitmq/rabbitmq-env.conf`:

```
NODE_IP_ADDRESS=127.0.0.1
```

Run the following command to restart RabbitMQ:

```sh
brew services restart rabbitmq
```
