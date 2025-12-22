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

/* ===== 视频嵌入样式 ===== */
.memo-video {
    position: relative;
    width: 100%;
    margin-top: 12px;
    border-radius: 8px;
    overflow: hidden;
    background: #000;
}

.memo-video-wrapper {
    position: relative;
    padding-bottom: 56.25%;
    height: 0;
}

.memo-video-wrapper iframe {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border: none;
}

.memo-video-cover {
    position: relative;
    cursor: pointer;
    aspect-ratio: 16/9;
    background-size: cover;
    background-position: center;
    border-radius: 8px;
    overflow: hidden;
}

.memo-video-cover::before {
    content: '';
    position: absolute;
    inset: 0;
    background: rgba(0,0,0,0.3);
    transition: background 0.2s;
}

.memo-video-cover:hover::before {
    background: rgba(0,0,0,0.1);
}

.memo-video-cover::after {
    content: '';
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 68px;
    height: 48px;
    background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 68 48"><path fill="%23f00" d="M66.52 7.74c-.78-2.93-2.49-5.41-5.42-6.19C55.79.13 34 0 34 0S12.21.13 6.9 1.55c-2.93.78-4.63 3.26-5.42 6.19C.06 13.05 0 24 0 24s.06 10.95 1.48 16.26c.78 2.93 2.49 5.41 5.42 6.19C12.21 47.87 34 48 34 48s21.79-.13 27.1-1.55c2.93-.78 4.63-3.26 5.42-6.19C67.94 34.95 68 24 68 24s-.06-10.95-1.48-16.26z"/><path fill="%23fff" d="M45 24 27 14v20"/></svg>') center/contain no-repeat;
}

.memo-video-cover.bilibili::after {
    background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 68 48"><rect fill="%2300a1d6" width="68" height="48" rx="8"/><path fill="%23fff" d="M45 24 27 14v20"/></svg>') center/contain no-repeat;
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
</style>

<script>
document.addEventListener("DOMContentLoaded", () => {
    const MEMOS_HOST = "https://memos.piio.me";
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

    // 提取视频信息
    function extractVideoInfo(text) {
        const videos = [];
        
        // YouTube
        const youtubePatterns = [
            /https?:\/\/(?:www\.)?youtube\.com\/watch\?v=([a-zA-Z0-9_-]{11})/g,
            /https?:\/\/youtu\.be\/([a-zA-Z0-9_-]{11})/g
        ];
        youtubePatterns.forEach(pattern => {
            let match;
            while ((match = pattern.exec(text)) !== null) {
                videos.push({
                    type: 'youtube',
                    id: match[1],
                    url: match[0],
                    thumbnail: `https://img.youtube.com/vi/${match[1]}/hqdefault.jpg`
                });
            }
        });
        
        // Bilibili
        const bilibiliPattern = /https?:\/\/(?:www\.)?bilibili\.com\/video\/(BV[a-zA-Z0-9]+)/g;
        let match;
        while ((match = bilibiliPattern.exec(text)) !== null) {
            videos.push({
                type: 'bilibili',
                id: match[1],
                url: match[0]
            });
        }
        
        return videos;
    }

    // 渲染视频嵌入
    function renderVideoEmbed(video) {
        if (video.type === 'youtube') {
            return `
                <div class="memo-video">
                    <div class="memo-video-cover" 
                         style="background-image: url('${video.thumbnail}')"
                         onclick="this.innerHTML='<div class=\\'memo-video-wrapper\\'><iframe src=\\'https://www.youtube.com/embed/${video.id}?autoplay=1\\' allow=\\'autoplay; encrypted-media\\' allowfullscreen></iframe></div>'; this.onclick=null; this.style.cursor='default';">
                    </div>
                </div>
            `;
        } else if (video.type === 'bilibili') {
            return `
                <div class="memo-video">
                    <div class="memo-video-wrapper">
                        <iframe src="//player.bilibili.com/player.html?bvid=${video.id}&page=1&high_quality=1&danmaku=0&autoplay=0" 
                                scrolling="no" allowfullscreen="true"></iframe>
                    </div>
                </div>
            `;
        }
        return '';
    }

    // 将视频链接原地替换为嵌入代码
    function replaceVideoUrls(text, videos) {
        let result = text;
        videos.forEach(video => {
            // 用占位符替换链接，稍后在HTML中替换为视频嵌入
            result = result.replace(video.url, `{{VIDEO_EMBED_${video.id}}}`);
        });
        return result;
    }

    // 渲染单条动态
    function renderMemo(memo) {
        const time = formatRelativeTime(memo.displayTime || memo.createTime);
        
        // 提取视频
        const videos = extractVideoInfo(memo.content || '');
        
        // 将视频链接替换为占位符
        let processedContent = replaceVideoUrls(memo.content || '', videos);
        
        // 解析 Markdown
        let content = marked.parse(processedContent);
        
        // 将占位符替换为实际的视频嵌入代码
        videos.forEach(video => {
            const embedHtml = renderVideoEmbed(video);
            content = content.replace(`{{VIDEO_EMBED_${video.id}}}`, embedHtml);
        });
        
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
        
        return `
            <div class="memo-card">
                <div class="memo-header">
                    ${tagsHtml}
                    <span class="memo-time">${time}</span>
                </div>
                <div class="memo-content">${content}</div>
                ${imagesHtml}
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
