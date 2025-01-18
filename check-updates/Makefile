PREFIX ?= /usr/local
SYSTEMD_DIR = /etc/systemd/system

all:
	@echo "Run 'make install WEBHOOK_URL=<url>' to install with your Slack webhook URL."

install:
	@if [ -z "$(WEBHOOK_URL)" ]; then \
		echo "Error: Please provide WEBHOOK_URL as an environment variable."; \
		exit 1; \
	fi
	# 一時ファイルを生成してスクリプトに Webhook URL を書き込む
	sed "s|__WEBHOOK_URL__|$(WEBHOOK_URL)|" check-updates.sh.template > /tmp/check-updates.sh
	install -Dm755 /tmp/check-updates.sh $(PREFIX)/bin/check-updates.sh
	rm -f /tmp/check-updates.sh
	install -Dm644 check-updates.service $(SYSTEMD_DIR)/check-updates.service
	install -Dm644 check-updates.timer $(SYSTEMD_DIR)/check-updates.timer
	systemctl daemon-reload
	systemctl enable check-updates.timer
	systemctl start check-updates.timer

uninstall:
	systemctl stop check-updates.timer
	systemctl disable check-updates.timer
	rm -f $(PREFIX)/bin/check-updates.sh
	rm -f $(SYSTEMD_DIR)/check-updates.service
	rm -f $(SYSTEMD_DIR)/check-updates.timer
	systemctl daemon-reload

.PHONY: all install uninstall
