<p>以下の内容は生成AIによって自動で作成されました。内容は必ずしも保証されておらず、詳細はコードを参照してください。</p>
<h1>実行順序</h1>

<div class="section">
<h2>Cosmic_Ray_Simulation.py</h2>
<p><strong>目的:</strong> 宇宙線が検出器を通過するシミュレーションを実行し、さまざまな検出器配置での同時計数率、立体角、および平均コサイン角度を計算します。結果は CosmicRaySimulation_flux.csv ファイルに保存されます。</p>
<p><strong>出力:</strong> CosmicRaySimulation/CosmicRaySimulation_flux.csv と simulation_config.cfg。</p>
<p><strong>注意事項:</strong> このスクリプトはシミュレーションの基盤となるデータを作成するため、必ず最初に実行する必要があります。シミュレーションパラメータはスクリプト内で直接設定されており、simulation_config.cfg にも出力されます。</p>
</div>

<div class="section">
<h2>add_mayoko.py</h2>
<p><strong>目的:</strong> 特定のシミュレーション結果（x=200, z=45）を CosmicRaySimulation_flux.csv に追加します。このスクリプトは既存のシミュレーション結果ファイルに依存しています。</p>
<p><strong>入力:</strong> CosmicRaySimulation/CosmicRaySimulation_flux.csv</p>
<p><strong>出力:</strong> 更新された CosmicRaySimulation/CosmicRaySimulation_flux.csv</p>
<p><strong>注意事項:</strong> このスクリプトは Cosmic_Ray_Simulation.py が実行され、CosmicRaySimulation_flux.csv が存在した後に実行する必要があります。</p>
</div>

<div class="section">
<h2>performance.py</h2>
<p><strong>目的:</strong> 検出器の性能（効率）を評価します。これは、異なるCOMポートからのデータを比較して、各検出器の相対的な効率を算出します。結果は performance.csv に保存され、後の分析で重み付けとして使用されます。</p>
<p><strong>入力:</strong> ./performance/datas/*.txt (実験データ)</p>
<p><strong>出力:</strong> ./performance/results/performance.csv</p>
<p><strong>注意事項:</strong> このスクリプトは、実際の検出器からのデータファイル (./performance/datas/ に格納されている.txtファイル) を必要とします。</p>
</div>

<div class="section">
<h2>ana_hist_date.py</h2>
<p><strong>目的:</strong> 生の実験データ（コインシデンスカウント）を読み込み、時間経過に伴うコインシデンスレートの平均と標準偏差を計算し、グラフとして出力します。また、バックグラウンド測定の結果を別途ファイルに保存します。</p>
<p><strong>入力:</strong> ./pocket_counter_output/datas/*.txt (実験データ)</p>
<p><strong>出力:</strong></p>
<ul>
  <li>./pocket_counter_output/images/ にレートの時系列グラフ (PNG)</li>
  <li>./pocket_counter_output/results/pocket_counter_output_*.csv に各測定の集計結果</li>
  <li>./pocket_counter_output/BG.csv にバックグラウンド測定の集計結果</li>
</ul>
<p><strong>注意事項:</strong> このスクリプトは、検出器の実験データが ./pocket_counter_output/datas/ ディレクトリに配置されていることを前提としています。バックグラウンド測定のデータもこのディレクトリに含まれている必要があります。</p>
</div>

<div class="section">
<h2>analyzation.py</h2>
<p><strong>目的:</strong> これまでのステップで生成されたシミュレーション結果、検出器効率、および実験データの集計結果を統合し、最終的な宇宙線フラックスの計算とコサイン角度に対するフィットを行います。結果はグラフとして出力され、Result.csv に保存されます。</p>
<p><strong>入力:</strong></p>
<ul>
  <li>./pocket_counter_output/results/*.csv (複数の pocket_counter_output_*.csv のうち最新のもの)</li>
  <li>./performance/results/performance.csv</li>
  <li>./CosmicRaySimulation/CosmicRaySimulation_flux.csv</li>
  <li>./pocket_counter_output/BG.csv</li>
</ul>
<p><strong>出力:</strong></p>
<ul>
  <li>Result.csv</li>
  <li>analyzation_result.png (フィット結果のグラフ)</li>
</ul>
<p><strong>注意事項:</strong> このスクリプトは、上記のすべての前提となるファイルが生成されていることを確認してから実行してください。</p>
</div>

<div class="section">
<h2>coin_change.py</h2>
<p><strong>目的:</strong> 各測定の累積平均コインシデンスレートとその標準偏差を時間経過でプロットします。これは、個々の測定の安定性や傾向を視覚的に確認するために使用できます。</p>
<p><strong>入力:</strong> ./pocket_counter_output/datas/*.txt (実験データ)</p>
<p><strong>出力:</strong> ./pocket_counter_output/changes/*.png</p>
<p><strong>注意事項:</strong> このスクリプトは、分析のメインフローとは独立して実行できますが、ana_hist_date.py と同じ実験データファイルを使用します。分析の補助的な可視化として利用できます。</p>
</div>

<div class="section">
<h2>全体的な注意事項</h2>
<p><strong>ディレクトリ構造:</strong> スクリプトは特定のディレクトリ構造を期待しています。実行前に以下のディレクトリが存在し、関連ファイルが適切に配置されていることを確認してください。</p>
<pre>
./CosmicRaySimulation/
./performance/datas/
./performance/results/
./pocket_counter_output/datas/
./pocket_counter_output/images/
./pocket_counter_output/results/
./pocket_counter_output/changes/
</pre>
<p><strong>ファイル名:</strong> スクリプトは glob と natsort を使用して最新のファイルを自動的に選択しますが、ファイル名の命名規則（特に日付やCOMポート情報を含むもの）がスクリプトの期待するものと一致していることを確認してください。</p>
<p><strong>データ形式:</strong> すべての入力データファイルはタブ区切り (\t) で、特定のヘッダーや列の順序に従っている必要があります。</p>
<p><strong>数値精度:</strong> スクリプト内で :.4e や :.4f のように浮動小数点数の出力精度が指定されている箇所があります。必要に応じて調整してください。</p>
</div>

</body>
</html>
