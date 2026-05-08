<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>世界史 - 古代文明の黎明</title>
    <style>
        /* ==========================================
           1. CSS変数 (カラーパレット & サイズ)
           古代遺跡や砂漠を連想させるアースカラーを基調とします。
        ========================================== */
        :root {
            --primary-color: #8B5A2B; /* 古代の土やレンガの色 */
            --secondary-color: #CD853F; /* 砂や黄金の色 */
            --bg-color: #FAF6ED; /* パピルスや古い紙のような背景色 */
            --text-main: #3E2723;
            --text-light: #5D4037;
            --accent-color: #4A148C; /* 権威や神秘性を表す紫 */
            --white: #FFFFFF;
            --shadow-md: 0 4px 10px rgba(139, 90, 43, 0.15);
            --shadow-hover: 0 8px 20px rgba(139, 90, 43, 0.25);
            --border-radius: 8px;
            --transition: 0.3s ease;
        }

        /* ==========================================
           2. 基本設定
        ========================================== */
        *, *::before, *::after {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Yu Mincho', 'Sawarabi Mincho', serif; /* 歴史的な雰囲気を出すために明朝体を採用 */
            background-color: var(--bg-color);
            color: var(--text-main);
            line-height: 1.8;
            /* 背景にうっすらとテクスチャ（ノイズ）を加える疑似的な表現 */
            background-image: radial-gradient(rgba(139, 90, 43, 0.03) 1px, transparent 1px);
            background-size: 20px 20px;
        }

        a {
            text-decoration: none;
            color: var(--primary-color);
        }

        /* ==========================================
           3. ヘッダー (Flexbox + パララックス風背景)
        ========================================== */
        header {
            background-color: var(--primary-color);
            color: var(--white);
            padding: 4rem 2rem;
            text-align: center;
            position: relative;
            overflow: hidden;
            box-shadow: 0 4px 12px rgba(0,0,0,0.2);
        }

        /* ヘッダー背景の装飾（CSSで描くピラミッド風の三角形） */
        header::before {
            content: '';
            position: absolute;
            bottom: -50px;
            right: 10%;
            width: 200px;
            height: 200px;
            background-color: rgba(255, 255, 255, 0.1);
            transform: rotate(45deg);
            z-index: 0;
        }
        
        header::after {
            content: '';
            position: absolute;
            bottom: -80px;
            left: 15%;
            width: 300px;
            height: 300px;
            background-color: rgba(0, 0, 0, 0.1);
            transform: rotate(45deg);
            z-index: 0;
        }

        header h1 {
            font-size: 3.5rem;
            margin-bottom: 1rem;
            position: relative;
            z-index: 1;
            letter-spacing: 0.1em;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.3);
        }

        header p {
            font-size: 1.2rem;
            max-width: 700px;
            margin: 0 auto;
            position: relative;
            z-index: 1;
            opacity: 0.9;
        }

        /* ==========================================
           4. ナビゲーション (Sticky)
        ========================================== */
        nav {
            position: sticky;
            top: 0;
            background-color: var(--white);
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
            z-index: 100;
        }

        nav ul {
            display: flex;
            justify-content: center;
            list-style: none;
            padding: 1rem;
            gap: 2rem;
        }

        nav a {
            font-weight: bold;
            color: var(--text-main);
            transition: color var(--transition);
            padding: 0.5rem;
            border-bottom: 2px solid transparent;
        }

        nav a:hover {
            color: var(--secondary-color);
            border-bottom-color: var(--secondary-color);
        }

        /* ==========================================
           5. メインコンテンツ領域
        ========================================== */
        .container {
            max-width: 1100px;
            margin: 0 auto;
            padding: 4rem 2rem;
        }

        .section-title {
            text-align: center;
            font-size: 2.5rem;
            color: var(--primary-color);
            margin-bottom: 3rem;
            position: relative;
        }

        .section-title::after {
            content: '〜';
            display: block;
            font-size: 1.5rem;
            color: var(--secondary-color);
            margin-top: 0.5rem;
        }

        /* ==========================================
           6. 文明カード (CSS Grid)
        ========================================== */
        .civilization-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
            gap: 2.5rem;
        }

        .card {
            background-color: var(--white);
            border-radius: var(--border-radius);
            padding: 2rem;
            box-shadow: var(--shadow-md);
            transition: all var(--transition);
            border-top: 5px solid var(--primary-color);
            position: relative;
            overflow: hidden;
        }

        .card:hover {
            transform: translateY(-10px);
            box-shadow: var(--shadow-hover);
        }

        /* カードごとのアクセントカラー変更 */
        .card.mesopotamia { border-top-color: #2E7D32; } /* 肥沃な三日月地帯の緑 */
        .card.egypt { border-top-color: #F57F17; } /* 太陽と砂漠の黄 */
        .card.indus { border-top-color: #1565C0; } /* 大河の青 */
        .card.china { border-top-color: #C62828; } /* 伝統的な赤 */

        .card h3 {
            font-size: 1.8rem;
            margin-bottom: 0.5rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .card-meta {
            font-size: 0.9rem;
            color: var(--secondary-color);
            font-weight: bold;
            margin-bottom: 1.5rem;
            display: block;
        }

        .card p {
            color: var(--text-light);
            margin-bottom: 1.5rem;
        }

        /* 箇条書きのカスタマイズ */
        .feature-list {
            list-style: none;
            padding-left: 0;
            border-top: 1px dashed rgba(0,0,0,0.1);
            padding-top: 1rem;
        }

        .feature-list li {
            position: relative;
            padding-left: 1.5rem;
            margin-bottom: 0.5rem;
            font-size: 0.95rem;
        }

        /* 独自のリストマーカー */
        .feature-list li::before {
            content: '❖';
            position: absolute;
            left: 0;
            color: var(--secondary-color);
            font-size: 0.8rem;
            top: 2px;
        }

        /* ==========================================
           7. タイムライン風セクション
        ========================================== */
        .timeline-section {
            margin-top: 6rem;
            background-color: var(--white);
            padding: 4rem 2rem;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow-md);
        }

        .timeline {
            position: relative;
            max-width: 800px;
            margin: 0 auto;
        }

        /* 中央の線 */
        .timeline::after {
            content: '';
            position: absolute;
            width: 4px;
            background-color: var(--secondary-color);
            top: 0;
            bottom: 0;
            left: 50%;
            transform: translateX(-50%);
        }

        .timeline-item {
            padding: 10px 40px;
            position: relative;
            background-color: inherit;
            width: 50%;
        }

        /* 丸いマーカー */
        .timeline-item::after {
            content: '';
            position: absolute;
            width: 20px;
            height: 20px;
            right: -10px;
            background-color: var(--white);
            border: 4px solid var(--primary-color);
            top: 15px;
            border-radius: 50%;
            z-index: 1;
        }

        .left { left: 0; }
        .right { left: 50%; }

        /* 右側のアイテムのマーカー位置を修正 */
        .right::after {
            left: -10px;
        }

        .timeline-content {
            padding: 1.5rem;
            background-color: var(--bg-color);
            border-radius: var(--border-radius);
            border: 1px solid rgba(0,0,0,0.05);
        }
        
        .timeline-date {
            color: var(--accent-color);
            font-weight: bold;
            margin-bottom: 0.5rem;
        }

        /* ==========================================
           8. フッター
        ========================================== */
        footer {
            background-color: var(--text-main);
            color: var(--white);
            text-align: center;
            padding: 2rem;
            margin-top: 4rem;
        }

        /* ==========================================
           9. レスポンシブ対応
        ========================================== */
        @media (max-width: 768px) {
            header h1 {
                font-size: 2.5rem;
            }

            nav ul {
                flex-wrap: wrap;
                gap: 1rem;
            }

            /* タイムラインを1列に変更 */
            .timeline::after {
                left: 31px;
            }
            .timeline-item {
                width: 100%;
                padding-left: 70px;
                padding-right: 25px;
            }
            .timeline-item::after {
                left: 21px;
            }
            .right {
                left: 0%;
            }
        }
    </style>
</head>
<body>

    <header>
        <h1>古代文明の黎明</h1>
        <p>人類の歴史の幕開け。大河のほとりで産声を上げた4つの偉大な文明の足跡を辿る。</p>
    </header>

    <nav>
        <ul>
            <li><a href="#mesopotamia">メソポタミア</a></li>
            <li><a href="#egypt">エジプト</a></li>
            <li><a href="#indus">インダス</a></li>
            <li><a href="#china">中国</a></li>
            <li><a href="#timeline">タイムライン</a></li>
        </ul>
    </nav>

    <div class="container">
        <h2 class="section-title">世界四大文明</h2>
        
        <div class="civilization-grid">
            
            <!-- メソポタミア文明 -->
            <article class="card mesopotamia" id="mesopotamia">
                <span class="card-meta">紀元前3500年頃〜</span>
                <h3>メソポタミア文明</h3>
                <p>ティグリス川とユーフラテス川の間に広がる「肥沃な三日月地帯」で栄えた、人類最古の文明の一つ。開放的な地形のため民族の興亡が激しく、シュメール人の都市国家（ウル、ウルク等）から始まり、アッカド人、そしてハンムラビ王を擁するアムル人（古バビロニア王国）へと次々に覇権が移り変わりました。</p>
                <p style="color: var(--text-light); margin-bottom: 1.5rem;">最古の文学作品とされる『ギルガメシュ叙事詩』が残されていることでも知られています。</p>
                <ul class="feature-list">
                    <li><strong>文字:</strong> 楔形文字（葦のペンで粘土板に記録）</li>
                    <li><strong>宗教:</strong> 多神教、都市ごとに守護神を祀るジッグラト（聖塔）を建設</li>
                    <li><strong>法律:</strong> ハムラビ法典（「目には目を、歯には歯を」の同害復讐法）</li>
                    <li><strong>科学・技術:</strong> 60進法（時間や角度の単位として現存）、太陰暦、車輪の発明</li>
                </ul>
            </article>

            <!-- エジプト文明 -->
            <article class="card egypt" id="egypt">
                <span class="card-meta">紀元前3000年頃〜</span>
                <h3>エジプト文明</h3>
                <p>「エジプトはナイルのたまもの」と呼ばれるように、ナイル川の定期的な氾濫がもたらす肥沃な土壌を基盤として発展しました。砂漠と海に囲まれた閉鎖的な地形であったため他民族の侵入が少なく、古王国・中王国・新王国と約3000年にわたり比較的安定した統一国家が継続しました。</p>
                <p style="color: var(--text-light); margin-bottom: 1.5rem;">死後の世界を強く信じており、『死者の書』などの副葬品やミイラ作りが発達しました。</p>
                <ul class="feature-list">
                    <li><strong>文字:</strong> 神聖文字（ヒエログリフ）、神官文字など。パピルス草の紙に記録</li>
                    <li><strong>政治:</strong> ファラオ（現人神・太陽神ラーの子）による絶対的な神権政治</li>
                    <li><strong>建築:</strong> ピラミッド（古王国時代の王墓）、王家の谷、スフィンクス</li>
                    <li><strong>科学:</strong> 太陽暦（氾濫時期を予測）、ナイル川氾濫後の土地復元のための測量術・幾何学</li>
                </ul>
            </article>

            <!-- インダス文明 -->
            <article class="card indus" id="indus">
                <span class="card-meta">紀元前2500年頃〜</span>
                <h3>インダス文明</h3>
                <p>インダス川流域に成立した、高度な都市計画を持つ文明。モヘンジョ・ダロやハラッパーなどの大規模な計画都市遺跡が発掘されています。</p>
                <ul class="feature-list">
                    <li><strong>文字:</strong> インダス文字（未解読）</li>
                    <li><strong>都市:</strong> 碁盤の目状の街路、下水道システム、大浴場</li>
                    <li><strong>社会:</strong> 巨大な王宮や神殿が見つかっておらず、比較的平等な社会であったと推測される</li>
                    <li><strong>衰退:</strong> 気候変動や環境破壊が原因とされる</li>
                </ul>
            </article>

            <!-- 中国文明 -->
            <article class="card china" id="china">
                <span class="card-meta">紀元前2000年頃〜</span>
                <h3>中国文明</h3>
                <p>黄河流域および長江流域で独自に発展した文明。神話上の夏王朝を経て、殷（商）、周へと王朝が受け継がれ、東アジア文化の源流となりました。</p>
                <ul class="feature-list">
                    <li><strong>文字:</strong> 甲骨文字（漢字の祖先）</li>
                    <li><strong>技術:</strong> 高度な青銅器の鋳造技術、絹織物</li>
                    <li><strong>思想:</strong> 祖先崇拝、天命思想</li>
                    <li><strong>政治:</strong> 封建制（周代）による統治</li>
                </ul>
            </article>

        </div>

        <div id="timeline" class="timeline-section">
            <h2 class="section-title">文明の夜明け 略年表</h2>
            <div class="timeline">
                
                <div class="timeline-item left">
                    <div class="timeline-content">
                        <div class="timeline-date">紀元前3500年頃</div>
                        <h4>シュメール人の都市国家成立</h4>
                        <p>メソポタミア南部にウル、ウルクなどの都市国家が誕生し、楔形文字が発明される。</p>
                    </div>
                </div>
                
                <div class="timeline-item right">
                    <div class="timeline-content">
                        <div class="timeline-date">紀元前3000年頃</div>
                        <h4>エジプト統一王朝の成立</h4>
                        <p>上下エジプトが統一され、初期王朝時代が始まる。ヒエログリフが使用され始める。</p>
                    </div>
                </div>
                
                <div class="timeline-item left">
                    <div class="timeline-content">
                        <div class="timeline-date">紀元前2500年頃</div>
                        <h4>インダス文明の繁栄</h4>
                        <p>モヘンジョ・ダロやハラッパーなどの計画都市が建設される。</p>
                    </div>
                </div>

                <div class="timeline-item right">
                    <div class="timeline-content">
                        <div class="timeline-date">紀元前1600年頃</div>
                        <h4>殷（商）王朝の成立</h4>
                        <p>中国最古の確認できる王朝。甲骨文字による占いが行われ、青銅器文化が栄える。</p>
                    </div>
                </div>

            </div>
        </div>

    </div>

    <footer>
        <p>&copy; 2026 古代文明史料館. All rights reserved.</p>
    </footer>

</body>
</html>
