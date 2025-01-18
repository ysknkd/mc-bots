PREFIX ?= /usr/local
SYSTEMD_DIR = /etc/systemd/system

all:
	@echo "Run 'make install WEBHOOK_URL=<url>' to install with your Slack webhook URL."

install:
	@if [ -z "$(WEBHOOK_URL)" ]; then \
		echo "Error: Please provide WEBHOOK_URL as an environment variable."; \
		exit 1; \
	fi
	# 一時ファイルを生成してスクリプトに Webhook URL を埋め込む
	sed "s|__WEBHOOK_URL__|$(WEBHOOK_URL)|" monitor-session.sh.template > /tmp/monitor-session.sh
	install -Dm755 /tmp/monitor-session.sh $(PREFIX)/bin/monitor-session.sh
	rm -f /tmp/monitor-session.sh
	install -Dm644 monitor-session.service $(SYSTEMD_DIR)/monitor-session.service
	systemctl daemon-reload
	systemctl enable monitor-session.service
	systemctl start monitor-session.service

uninstall:
	systemctl stop monitor-session.service
	systemctl disable monitor-session.service
	rm -f $(PREFIX)/bin/monitor-session.sh
	rm -f $(SYSTEMD_DIR)/monitor-session.service
	systemctl daemon-reload

.PHONY: all install uninstall
