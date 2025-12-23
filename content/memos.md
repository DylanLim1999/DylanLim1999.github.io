---
title: "碎碎念"
date: 2025-12-21
layout: "single"
---

<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>

<content>

<div id="memos-list" class="memos-container">
    <p class="memos-loading">正在加载动态...</p>
</div>

<div id="load-more-container" style="text-align: center; margin: 20px 0; display: none;">
    <button id="load-more-btn" class="load-more-btn">加载更多</button>
</div>

</content>

<style>
/* ===== 容器样式 ===== */
.memos-container {
    max-width: 720px;
    margin: 0 auto;
    padding-top: 16px;
}

.memos-loading {
    text-align: center;
    color: var(--text-color-tertiary, #888);
}

/* ===== 卡片样式 ===== */
.memo-card {
    border: 1px solid var(--border-color, #e5e7eb);
    border-radius: 12px;
    padding: 20px;
    margin-bottom: 20px;
    background: var(--bg-color-secondary, #f9fafb);
    transition: box-shadow 0.2s ease;
}

.memo-card:hover {
    box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}

/* ===== 头部信息 ===== */
.memo-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
    flex-wrap: wrap;
    gap: 8px;
}

.memo-time {
    font-size: 0.85em;
    color: var(--text-color-tertiary, #9ca3af);
}

/* ===== 标签样式 ===== */
.memo-tags {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
}

.memo-tag {
    font-size: 0.75em;
    padding: 2px 8px;
    border-radius: 12px;
    background: var(--link-color, #028760);
    color: #fff;
    text-decoration: none;
}

/* ===== 内容区域 ===== */
.memo-content {
    font-size: 1rem;
    line-height: 1.75;
    color: var(--text-color-primary, #374151);
    word-wrap: break-word;
}

.memo-content p {
    margin: 0.5em 0;
}

.memo-content p:first-child {
    margin-top: 0;
}

.memo-content p:last-child {
    margin-bottom: 0;
}

/* ===== 图片样式 ===== */
.memo-images {
    display: flex;
    flex-direction: row;
    gap: 8px;
    margin-top: 12px;
    overflow-x: auto;
    overflow-y: hidden;
    padding-bottom: 4px;
    scrollbar-width: thin;
    scrollbar-color: var(--border-color, #e5e7eb) transparent;
}

.memo-images::-webkit-scrollbar {
    height: 6px;
}

.memo-images::-webkit-scrollbar-track {
    background: transparent;
}

.memo-images::-webkit-scrollbar-thumb {
    background: var(--border-color, #e5e7eb);
    border-radius: 3px;
}

.memo-images img {
    flex-shrink: 0;
    height: 160px;
    width: auto;
    max-width: 280px;
    border-radius: 8px;
    cursor: pointer;
    object-fit: cover;
    border: 1px solid var(--border-color, #e5e7eb);
}

.memo-images.single img {
    height: auto;
    max-height: 400px;
    max-width: 100%;
    object-fit: contain;
}

.memo-content img {
    max-width: 100%;
    height: auto;
    border-radius: 8px;
    margin: 10px 0;
    display: block;
    border: 1px solid var(--border-color, #e5e7eb);
}


/* ===== 加载更多按钮 ===== */
.load-more-btn {
    padding: 10px 24px;
    border: 1px solid var(--border-color, #e5e7eb);
    border-radius: 8px;
    background: var(--bg-color-secondary, #f9fafb);
    color: var(--text-color-primary, #374151);
    cursor: pointer;
    font-size: 0.9em;
    transition: all 0.2s ease;
}

.load-more-btn:hover {
    background: var(--bg-color-primary, #fff);
    border-color: var(--link-color, #028760);
    color: var(--link-color, #028760);
}

.load-more-btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}

/* ===== 空状态和错误状态 ===== */
.memos-empty, .memos-error {
    text-align: center;
    padding: 40px 20px;
}

.memos-empty {
    color: var(--text-color-tertiary, #888);
}

.memos-error {
    color: #ef4444;
}

/* ===== 链接预览卡片样式 ===== */
.link-preview {
    display: flex;
    align-items: stretch;
    border: 1px solid var(--border-color, #e5e7eb);
    border-radius: 12px;
    overflow: hidden;
    margin-top: 12px;
    background: var(--bg-color-primary, #fff);
    text-decoration: none;
    color: inherit;
    transition: all 0.2s ease;
    height: 90px;
}

.link-preview:hover {
    border-color: var(--link-color, #028760);
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    transform: translateY(-1px);
}

.link-preview-image {
    flex-shrink: 0;
    height: 100%;
    aspect-ratio: 1 / 1;
    object-fit: cover;
    background: linear-gradient(135deg, var(--bg-color-secondary, #f3f4f6) 0%, var(--border-color, #e5e7eb) 100%);
    display: block;
    border-radius: 0;
    border: none !important;
}

.link-preview-content {
    flex: 1;
    padding: 10px 14px;
    min-width: 0;
    display: flex;
    flex-direction: column;
    justify-content: center;
    gap: 2px;
    overflow: hidden;
}

.link-preview-title {
    font-size: 0.88em;
    font-weight: 600;
    color: var(--text-color-primary, #374151);
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    line-height: 1.3;
}

.link-preview-description {
    font-size: 0.78em;
    color: var(--text-color-tertiary, #6b7280);
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    line-height: 1.35;
}

.link-preview-site {
    font-size: 0.72em;
    color: var(--link-color, #028760);
    display: flex;
    align-items: center;
    gap: 4px;
}

.link-preview-site::before {
    content: '🔗';
    font-size: 0.85em;
}

.link-preview.loading {
    height: auto;
    padding: 20px;
    justify-content: center;
    color: var(--text-color-tertiary, #9ca3af);
    font-size: 0.85em;
    background: var(--bg-color-secondary, #f9fafb);
}

.link-preview.loading::before {
    content: '';
    width: 16px;
    height: 16px;
    border: 2px solid var(--border-color, #e5e7eb);
    border-top-color: var(--link-color, #028760);
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
    margin-right: 8px;
}

@keyframes spin {
    to { transform: rotate(360deg); }
}

.link-preview.error {
    display: none;
}

/* 无图片时的样式 */
.link-preview.no-image {
    background: linear-gradient(135deg, var(--bg-color-primary, #fff) 0%, var(--bg-color-secondary, #f9fafb) 100%);
}

.link-preview.no-image .link-preview-content {
    padding: 14px 18px;
}

/* 响应式 */
@media (max-width: 480px) {
    .link-preview {
        height: 80px;
    }
    .link-preview-content {
        padding: 8px 12px;
    }
    .link-preview-title {
        font-size: 0.82em;
    }
    .link-preview-description {
        font-size: 0.72em;
        -webkit-line-clamp: 1;
    }
}
</style>

<script>
document.addEventListener("DOMContentLoaded", () => {
    const MEMOS_HOST = "https://memos.piio.me";
    const LINK_PREVIEW_API = "https://link-preview.piio.me";
    const PAGE_SIZE = 20;
    
    let nextPageToken = "";
    let isLoading = false;
    
    const container = document.getElementById('memos-list');
    const loadMoreContainer = document.getElementById('load-more-container');
    const loadMoreBtn = document.getElementById('load-more-btn');

    // 相对时间格式化
    function formatRelativeTime(dateStr) {
        const date = new Date(dateStr);
        const now = new Date();
        const diff = now - date;
        
        const seconds = Math.floor(diff / 1000);
        const minutes = Math.floor(seconds / 60);
        const hours = Math.floor(minutes / 60);
        const days = Math.floor(hours / 24);
        
        if (days > 7) {
            return date.toLocaleDateString('zh-CN', {
                year: 'numeric', month: '2-digit', day: '2-digit'
            });
        } else if (days > 0) {
            return `${days} 天前`;
        } else if (hours > 0) {
            return `${hours} 小时前`;
        } else if (minutes > 0) {
            return `${minutes} 分钟前`;
        } else {
            return '刚刚';
        }
    }

    // 处理 Memos 资源链接
    function processResources(memo) {
        let images = [];
        if (memo.attachments && memo.attachments.length > 0) {
            memo.attachments.forEach(att => {
                if (att.type && att.type.startsWith('image/')) {
                    const resourceUrl = `${MEMOS_HOST}/file/${att.name}/${att.filename}`;
                    images.push(resourceUrl);
                }
            });
        }
        return images;
    }

    // 提取所有链接（排除图片）
    function extractLinks(text) {
        // 匹配 URL
        const urlPattern = /https?:\/\/[^\s<>"'\)\]]+/g;
        const matches = text.match(urlPattern) || [];
        
        // 过滤掉图片链接
        const imageExtensions = ['.jpg', '.jpeg', '.png', '.gif', '.webp', '.svg', '.ico', '.bmp'];
        return [...new Set(matches)].filter(url => {
            const lowerUrl = url.toLowerCase();
            return !imageExtensions.some(ext => lowerUrl.includes(ext));
        });
    }

    // 从内容中移除链接
    function removeLinksFromContent(text, links) {
        let result = text;
        links.forEach(url => {
            // 移除裸链接
            result = result.replace(new RegExp(url.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'), 'g'), '');
        });
        // 清理空的 Markdown 链接标记 []()
        result = result.replace(/\[([^\]]*)\]\(\s*\)/g, '$1');
        // 清理多余的空行
        result = result.replace(/\n{3,}/g, '\n\n');
        return result.trim();
    }

    // 获取链接预览数据
    async function fetchLinkPreview(url) {
        try {
            const response = await fetch(`${LINK_PREVIEW_API}/?url=${encodeURIComponent(url)}`);
            if (!response.ok) throw new Error('Failed to fetch');
            return await response.json();
        } catch (error) {
            console.warn('Link preview failed for:', url, error);
            return null;
        }
    }

    // 渲染链接预览卡片
    function renderLinkPreviewPlaceholder(url, index) {
        return `<a href="${url}" target="_blank" rel="noopener noreferrer" class="link-preview loading" data-preview-url="${url}" data-preview-index="${index}">加载链接预览...</a>`;
    }

    // 更新链接预览卡片
    function updateLinkPreview(element, data) {
        if (!data || data.error) {
            element.classList.remove('loading');
            element.classList.add('error');
            return;
        }
        
        const hasImage = data.image && data.image.length > 0;
        element.classList.remove('loading');
        if (!hasImage) element.classList.add('no-image');
        
        element.innerHTML = `
            ${hasImage ? `<img class="link-preview-image" src="${data.image}" alt="" loading="lazy" onerror="this.style.display='none';this.parentElement.classList.add('no-image');">` : ''}
            <div class="link-preview-content">
                <div class="link-preview-title">${data.title || data.url}</div>
                ${data.description ? `<div class="link-preview-description">${data.description}</div>` : ''}
                <div class="link-preview-site">${data.siteName || new URL(data.url).hostname}</div>
            </div>
        `;
    }

    // 渲染单条动态
    function renderMemo(memo) {
        const time = formatRelativeTime(memo.displayTime || memo.createTime);
        
        // 提取所有链接
        const links = extractLinks(memo.content || '');
        
        // 从内容中移除链接
        let processedContent = removeLinksFromContent(memo.content || '', links);
        
        // 解析 Markdown
        let content = marked.parse(processedContent);
        
        // 处理图片资源
        const images = processResources(memo);
        
        // 渲染标签
        let tagsHtml = '';
        if (memo.tags && memo.tags.length > 0) {
            tagsHtml = `<div class="memo-tags">
                ${memo.tags.map(tag => `<span class="memo-tag">#${tag}</span>`).join('')}
            </div>`;
        }
        
        // 渲染图片
        let imagesHtml = '';
        if (images.length > 0) {
            const gridClass = images.length === 1 ? 'single' : 'multiple';
            imagesHtml = `<div class="memo-images ${gridClass}">
                ${images.map(img => `<img src="${img}" alt="" loading="lazy">`).join('')}
            </div>`;
        }
        
        // 渲染链接预览占位符（只预览第一个链接）
        let linkPreviewsHtml = '';
        if (links.length > 0) {
            linkPreviewsHtml = renderLinkPreviewPlaceholder(links[0], 0);
        }
        
        return `
            <div class="memo-card">
                <div class="memo-header">
                    ${tagsHtml}
                    <span class="memo-time">${time}</span>
                </div>
                <div class="memo-content">${content}</div>
                ${imagesHtml}
                ${linkPreviewsHtml}
            </div>
        `;
    }

    // 加载动态
    async function loadMemos(isLoadMore = false) {
        if (isLoading) return;
        isLoading = true;
        
        if (!isLoadMore) {
            container.innerHTML = '<p class="memos-loading">正在加载动态...</p>';
        } else {
            loadMoreBtn.disabled = true;
            loadMoreBtn.textContent = '加载中...';
        }
        
        try {
            let apiUrl = `${MEMOS_HOST}/api/v1/memos?pageSize=${PAGE_SIZE}&filter=visibility=='PUBLIC'`;
            if (nextPageToken) {
                apiUrl += `&pageToken=${nextPageToken}`;
            }
            
            const response = await fetch(apiUrl);
            const data = await response.json();
            
            const memos = data.memos || [];
            nextPageToken = data.nextPageToken || "";
            
            if (memos.length === 0 && !isLoadMore) {
                container.innerHTML = '<p class="memos-empty">暂时没有公开的动态 📝</p>';
                loadMoreContainer.style.display = 'none';
                return;
            }
            
            const html = memos.map(renderMemo).join('');
            
            if (isLoadMore) {
                container.insertAdjacentHTML('beforeend', html);
            } else {
                container.innerHTML = html;
            }
            
            // 异步加载链接预览
            const previewElements = container.querySelectorAll('.link-preview.loading');
            previewElements.forEach(async (element) => {
                const url = element.dataset.previewUrl;
                const data = await fetchLinkPreview(url);
                updateLinkPreview(element, data);
            });
            
            if (nextPageToken) {
                loadMoreContainer.style.display = 'block';
                loadMoreBtn.disabled = false;
                loadMoreBtn.textContent = '加载更多';
            } else {
                loadMoreContainer.style.display = 'none';
            }
            
        } catch (err) {
            console.error('Failed to load memos:', err);
            if (!isLoadMore) {
                container.innerHTML = '<p class="memos-error">加载失败，请稍后重试 😢</p>';
            }
        } finally {
            isLoading = false;
        }
    }

    loadMoreBtn.addEventListener('click', () => loadMemos(true));
    loadMemos();
});
</script>
